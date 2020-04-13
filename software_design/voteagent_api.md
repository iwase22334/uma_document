# voteagent api

## request sample

{
    "race_id": {
        "year": "2018",
        "monthday":"1001",
        "jyocd":"01",
        "kaiji":"01",
        "nichiji": "01",
        "racenum": "01"
}

## response sample

{
    "tansyo": [
        { "01": 0.11 },
        { "02": 0.23 },
        { "03": 0.02 },
        { "04": 0.08 },
        ...
    ]
}

## error response sample

{
    "error": "Record not found"
}
