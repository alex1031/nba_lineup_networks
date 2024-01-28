# Analysing NBA Player Performance using Network Theory
This project aims to apply network theory concepts to analyse player performance based on the quality of lineups they were involved in, and hence determine the best players in each team, the league, and compare to popular advanced statistics used today. The use of networks allows for a more interpretable result on the impact a player has on the team relative to their teammates. This provides a different perspective on player performance, as current advanced statistics represent each player as their own problem space. This is a recreation of the paper by 

Lineup data used in this project were from the 2022-23 NBA season.

## Method

### Data Collection
Data required for this project involved per game statistics for each player and five-man lineup. In order to avoid (insignificant) lineups being included in the model, (i.e. Garbage time lineups), lineups with less than two minutes were excluded. Similarly, lineups with less than five games played were filtered out. These conditions filtered out lineups with extreme statistics due to the lack of play time and sample size. All data in this project were retrieved from the python api module nba_api [link]. 

The final data set involved 539 players and 1219 lineups.  

### Model
From the dataset, a bipartite network for each team was constructed where nodes were either players or lineups. Weighted edges were then created between players and lineups, defined by metrics covering different offensive and defensive aspects of the game. Such metrics include:
-	Field-Goal Percentage
-	Assist to Turnover Ratio
-	Offensive Rebound Percentage
-	Defensive Rebound Percentage
-	Steals/Possessions ratio
-	Opponent Field-Goal Miss Percentage

To adjust for a player’s involvement in more important lineups, defined by how much that lineup was utilised during the season, a ratio between the specific lineup minutes and average team lineup minutes was introduced as a multiplier for a player’s per game stats. Furthermore, each lineup metric was standardised to their respective teams, serving as another adjustment multiplier for players that were involved in lineups with high performance. Ultimately, each edge weighing function can represented with the following formula:

$$ Weight = M*(\frac{LM}{Tm AVG LM}) * (MStd + 1)$$

where M is the metric, LM is lineup minutes, and MStd is the standardised metric. The plus one was required since the variable’s mean and standard deviation are both set to 0.
All network building was implemented through the Python module NetworkX.

### Evaluation Metric

PageRank and Eigenvector Centrality were chosen as the node centrality algorithms to evaluate the importance of each player within the networks. PageRank measures the most important nodes in a network using in-links and out-links, while Eigenvector Centrality is a measure of both how well-connected a node is, as well as the quality of the connection. Full mathematical definitions can be found here [link]. These were also implemented through NetworkX.

