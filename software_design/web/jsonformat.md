# json format

## time_predictor 

### request
```
{
    "race_id": {
        "year": "2018",
        "monthday":"1001",
        "jyocd":"01",
        "kaiji":"01",
        "nichiji": "01",
        "racenum": "01"
    },
}
``` 

### response
```
{
    "bins": 150
    "width": 0.3
    "dataset": [
        { 
            "umaban": "03",
            "name": "シタヤマゴーゴー",
            "pdf": [
                {x:64.1,y:0.00},
                {x:64.2,y:0.06},
                {x:64.3,y:0.011},
                ...
                {x:78.8,y:0.00},
                ]
        },
        ...
    ]

}
```

## winprob_estimator

### request
```
{
    "bins": 150
    "width": 0.3
    "dataset": [
        { 
            "umaban": "03",
            "name": "シタヤマゴーゴー",
            "pdf": [
                {x:64.1,y:0.00},
                {x:64.2,y:0.06},
                {x:64.3,y:0.011},
                ...
                {x:78.8,y:0.00},
                ]
        },
        ...
    ]

}
```

### response

```
{
    "tansyo": [
        { 
          "umaban": "01",
          "name": "シタヤマゴーゴー",
          "prob" : 0.11 
        },
        ...
    ]
}
```

### time_prediction.json

```
const _datapack = {
    'data' : [
        {
            'date'  : '20201201',
            'race_info' : [ 
                {
                    'venue' : '東京',
                    'races' : [ 
                        {
                            'id'         : '2020040101010101',
                            'round'      : '01',
                            'name'       : '下山特別',
                            'jyoken'     : '３歳未勝利',
                            'track'      : 'ダート',
                            'kyori'      : '1200'
                            ''
                        },
                        {
                            'id'         : '2020040101010102',
                            'round'      : '02',
                            'name'       : '',
                            'jyoken'     : '３歳未勝利',
                            'kyori'      : '1200'
                        },
                        {
                            'id'         : '2020040101010103',
                            'round'      : '03',
                            'name'       : '',
                            'jyoken'     : '３歳未勝利',
                            'track'      : 'ダート',
                            'kyori'      : '1200'
                        }
                    ]
                },
                {
                    'venue' : '阪神',
                    'races' : [ 
                        {
                            'id'         : '2020040101010201',
                            'round'      : '01',
                            'name'       : '',
                            'jyoken'     : '３歳未勝利',
                            'track'      : 'ダート',
                            'kyori'      : '1200'
                        },
                        {
                            'id'         : '2020040101010202',
                            'round'      : '02',
                            'name'       : '',
                            'jyoken'     : '３歳未勝利',
                            'course'     : 'ダート・右 1200m',
                        },
                        {
                            'id'         : '2020040101010203',
                            'round'      : '03',
                            'name'       : '',
                            'jyoken'     : '３歳未勝利',
                            'course'     : 'ダート・右 1200m',
                        }
                    ]
                },
            ]
        }
    ]
};

function get_datapack() { 
    return _datapack;
}


```