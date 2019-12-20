@startuml

database "everydb2" {
    component "raw data" as rawdata 
}

database "uma_processed" {
    component "processed data" as processeddata 
}

interface "COM" as jv_link_if
[jv_link] - jv_link_if 

[everydb2] ..> jv_link_if : "Request Race data"
[everydb2] -> [rawdata] : "everydb2->postgres\nJRA-VAN raw data"

package "jra_connector" {
    [event_crawler] ..> jv_link_if : "Request realtime data.\n(RaceInfo, Odds, etc."
}

package "uma_agent" {
    component "proxy" as proxy

    interface "HTTP/JSON" as agent_if

    folder {
        component "model, parameter" as model
    }

    [trainer] --> [model] : Save data.

    [trainer] ..> [proxy] : Request data
    [winprob_estimator] ..> [proxy] : Request data

    [winprob_estimator] - agent_if 
    [winprob_estimator] ..> [model] : Load data.
}

package "tester" { 
    interface "HTTP/JSON" as simulator_if
    [dummy_race] - simulator_if
    [dummycoordinator]
    [dummycoordinator] ..> simulator_if : "Vote and get payout" 
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

[dummycoordinator] <- [rawdata]
[dummycoordinator] ..> agent_if : "Request vote with\n passing the race data"

[rawdata] -> [proxy]
[processeddata] -> [proxy]

component "Data analyzer" as dataanalyzer
[rawdata] --> [dataanalyzer]
[dataanalyzer] --> [processeddata]


@enduml