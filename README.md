# The Spades API

This repository provides an API for storing and interacting with spades games.
It is written in Go and developed to be [JSON API](http://jsonapi.org/) compliant.

## API Reference

This section provides an overview of the main resources and routes used in the
Spades API. Interactions are done via standard HTTP calls with JSON and uses the
JSON API conventions.

### Resource `/players`

#### `POST /players` Request

````
{
  data: {
    type: 'players',
    attributes: {
      name: 'John Wang',
    },
  },
}
````

#### `/players` Response

````
{
  data: {
    type: 'players',
    id: 'player_id1',
    attributes: {
      name: 'John Wang',
    },
    links: {
      self: '/players/player_id1',
    },
  },
}
````

### Resource `/games`

#### `POST /games` Request

````
{
  data: {
    type: 'games',
    attributes: {
      order: [player_id1, player_id3, player_id2, player_id4],
      play_until: 500,
    },
    relationships: {
      teams: {
        data: [{
          type: 'teams',
          attributes: {
            player_1: player_id1,
            player_2: player_id2,
          },
        }, {
          type: 'teams',
          attributes: {
            player_1: player_id3,
            player_2: player_id4,
          },
        }],
      },
    },
  },
}
````

#### `/games` Response

````
{
  data: {
    type: 'games',
    id: 'game1',
    attributes: {
      order: [player_id1, player_id2, player_id3, player_id4], 
      play_until: 500,
      winner: null,
    },
    links: {
      self: '/games/game1',
    },
    relationships: {
      rounds: {
        links: {
          self: '/games/game1/rounds',
        },
        data: [],
      },
      teams: {
        links: {
          self: '/games/game1/teams',
        },
        data: [{
          type: 'teams',
          id: 'team_id1',
          attributes: {
            player_1: 'player_id1',
            player_2: 'player_id2',
          },
        }, {
          type: 'teams',
          id: 'team_id2',
          attributes: {
            player_1: 'player_id3',
            player_2: 'player_id4',
          },
        }]
      }
    },
  },
}
````

#### `GET /games/{game_id}/rounds` Response

````
{
  data: [{
    type: 'rounds',
    id: 'round1',
    attributes: {
      bidding_order: ['player_id1', 'player_id3', 'player_id2', 'player_id4'],
      playing_order: ['player_id1', 'player_id3', 'player_id2', 'player_id4'],
      bids: [],
      tricks: [],
    },
    relationships: {
      games: {
        data: {
          type: 'games',
          id: 'game1',
        },
        links: {
          self: '/games/game1',
        },
      },
      scores: {
        data: [{
          type: 'scores',
          id: 'score1',
          attributes: {
            team: 'team1',
            round_score: null,
            total_score: 0,
          },
        }, {
          type: 'scores',
          id: 'score2',
          attributes: {
            team: 'team2',
            round_score: null,
            total_score: 0,
          },
        }],
        links: {
          self: '/games/game1/rounds/round1/scores',
        },
      },
    },
  }],
  links: {
    self: '/games/game1/rounds',
  },
}
````

### `/teams` Resource

#### `POST /teams` Request

````
{
  data: {
    type: 'teams',
    attributes: {
      player_1: 'player_id1',
      player_2: 'player_id2',
    },
  },
}
````

#### `/teams` Response

````
{
  data: {
    type: 'teams',
    id: 'team1',
    attributes: {
      player_1: 'player_id1',
      player_2: 'player_id2',
    },
    links: {
      self: '/teams/team1',
    },
  },
}
````
