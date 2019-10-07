@startuml

database "everydb2" {
    component "raw data" as rawdata 
}

database "uma_processed" {
    component "processed data" as processeddata 
}

database "uma_bulletin" {
    component "Bulletin data" as bullet 
}

database "uma_log" {
    [log]
}


interface "COM" as jv_link_if
[jv_link] - jv_link_if 

[everydb2] ..> jv_link_if : "Request Race data"
[everydb2] -> [rawdata] : "everydb2->postgres\nJRA-VAN raw data"

package "jra_connector" {
    [event_crawler] ..> jv_link_if : "Request realtime data.\n(RaceInfo, Odds, etc."
}

package "uma_agent" {
    component "Dataset Proxy" as proxy

    interface "HTTP/JSON" as agent_if
    note right
      Desigined according to REST API.
      API is designed not to depend on
      parameter set that used by agent. 
    end note

    folder {
        component "model, parameter" as model
    }

    [trainer] --> [model] : Save data.

    [trainer] ..> [proxy] : Request data
    [vote_agent] ..> [proxy] : Request data

    [vote_agent] - agent_if 
    [vote_agent] ..> [model] : Load data.
}

package "tester" { 
    interface "HTTP/JSON" as simulator_if
    note right: Desigined according to REST API.\n Easy to access by using \n uma_dummyrace_client.
    [dummy_race] - simulator_if
    [dummy_crawler]
    [dummy_crawler] ..> simulator_if : "Vote and get payout" 
    [dummy_race] <- [rawdata]
}

cloud {
    [ipat]
}

cloud {
    [JRA]
}

[jv_link] <--> [JRA]
[event_crawler] ..> agent_if: "Request race result with\n passing the vote"
[event_crawler] --> [ipat]
[event_crawler] -> [log]
[dummy_crawler] -> [log]

[dummy_crawler] <- [rawdata]
[dummy_crawler] ..> agent_if : "Request vote with\n passing the race data"

[rawdata] -> [proxy]
[processeddata] -> [proxy]

component "Data analyzer" as dataanalyzer
[rawdata] --> [dataanalyzer]
[dataanalyzer] --> [processeddata]


@enduml