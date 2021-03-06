
## Data Scraping

### Note
Uses python3.6

### Usage
In order to obtain the entire 2017 KBO baseball data, simply run the following.
```python
from scrape import *
year = '2015'
months = ['03', '04', '05', '06', '07', '08', '09', '10']
summaries = []
for month in months:
    summaries += MatchSummaryParser(year, month).parse()

matches = []
for summary in summaries:
    try:
        matches.append(
            MatchDetailParser(
                summary.year,
                summary.month,
                summary.day,
                summary.get_away_team_name(),
                summary.get_home_team_name()
            ).parse()
        )
    except DetailDataNotFoundException:
        # this is most likely a double header game.
        pass
```

### Baseball Terms Explained

summary.r : 점수

summary.b : 볼넷 + 사사구

summary.e : 팀 실수

summary.h : 안타

pitcher_info.at_bats : 상대한 타자 수

pitcher_info.hits : 맞은 안타 수

pitcher_info.bbhp : 던진 볼넷 + 사사구

pitcher_info.home_runs : 맞은 홈런 수

pitcher_info.strike_outs : 잡은 스트라이크 아웃 수

pitcher_info.at_bats : 상대한 타자 수

pitcher_info.scores_lost : 투수가 내준 점수

pitcher_info.errors : 투수 실수

pitcher_info.era : 투수 방어율

pitcher_info.innings : 던진 이닝 수. 단, 내림 처리 하였기 때문에 3 1/2, 2/3 모두 3으로 내림.

pitcher_info.wins : 투수의 시즌 승수

pitcher_info.loses : 투수의 시즌 패배 수

pitcher_info.saves : 투수의 시즌 세이브 수

pitcher_info.num_balls_thrown : 투수가 해당 경기 던진 공 수

pitcher_info.game_count : 투수의 시즌 등판 수

batter_info.at_bats : 타자가 해당 경기에 타석에 들어간 횟 수

batter_info.hits : 타자의 해당 게임 안타 수

batter_info.hra : 타자의 해당 경기 종료 시점의 타율

batter_info.rbi : 타자의 해당 경기 타점 (때려서 홈으로 불러 들여온 주자 수)

batter_info.runs : 타자의 해당 경기 득점 (본인이 홈을 밟은 수)

team_standing.draws : 종료 시점의 해당 팀의 무승부 수

team_standing.era : 종료 시점의 팀의 평균 방어율

team_standing.hra : 종료 시점의 팀의 평균 타율

team_standing.wra : 종료 시점의 팀의 평균 승률

team_standing.wins : 종료 시점의 팀의 승수

team_standing.loses : 종료 시점의 팀의 패수

team_standing.rank : 종료 시점의 팀의 랭킹


### For example
some specific game's data (with some pitchers and batters omitted for brevity)

```json
{
    "year": "2017",
    "month": "03",
    "day": "14",
    "away_team_name": "KT",
    "home_team_name": "SAMSUNG",
    "score_board": {
      "scores": {
        "away": [
          3,
          1,
          1,
          0,
          1,
          0,
          2,
          0,
          1
        ],
        "home": [
          0,
          0,
          0,
          0,
          1,
          0,
          0,
          0,
          0
        ]
      },
      "summary": {
        "away": {
          "r": 9,
          "b": 6,
          "e": 0,
          "h": 12
        },
        "home": {
          "r": 1,
          "b": 3,
          "e": 0,
          "h": 7
        }
      }
    },
    "pitcher_info": {
      "home": [
        {
          "at_bats": 11,
          "hits": 6,
          "bbhp": 3,
          "home_runs": 0,
          "strike_outs": 1,
          "scores_lost": 5,
          "errors": 5,
          "era": "15.00",
          "name": "\ucd5c\ucda9\uc5f0",
          "innings": "3",
          "wins": 0,
          "loses": 1,
          "saves": 0,
          "num_balls_thrown": 60,
          "game_count": 1
        }
      ],
      "away": [
        {
          "at_bats": 18,
          "hits": 6,
          "bbhp": 1,
          "home_runs": 0,
          "strike_outs": 1,
          "scores_lost": 1,
          "errors": 1,
          "era": "1.80",
          "name": "\ub85c\uce58",
          "innings": "5",
          "wins": 1,
          "loses": 0,
          "saves": 0,
          "num_balls_thrown": 72,
          "game_count": 1
        }
      ]
    },
    "batter_info": {
      "home": [
        {
          "at_bats": 2,
          "hits": 1,
          "hra": "0.500",
          "rbi": 1,
          "runs": 0,
          "name": "\ubc15\ud574\ubbfc"
        }
      ],
      "away": [
        {
          "at_bats": 3,
          "hits": 1,
          "hra": "0.333",
          "rbi": 0,
          "runs": 1,
          "name": "\uc774\ub300\ud615"
        }
      ]
    },
    "away_team_standing": {
      "draws": 0,
      "era": 5.53,
      "hra": 0.264,
      "wra": 0.373,
      "wins": 25,
      "loses": 42,
      "rank": 9,
      "name": "KT"
    },
    "home_team_standing": {
      "draws": 2,
      "era": 5.81,
      "hra": 0.265,
      "wra": 0.369,
      "wins": 24,
      "loses": 41,
      "rank": 10,
      "name": "SAMSUNG"
    }
  }
]
```
