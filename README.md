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

## Results

Results can be found in the results excel file above. These results can also be recreated through using the Network Theory notebook.

### Best Players by Teams 

The results from the network will be compared against advanced statistics used in the league today, specifically PER (Player Efficiency Rating), WS/48 (Win Share per 48) and VORP (Value Over Replacement player). 

For each team, the model output is counted as successful if the player appeared in any of the advanced statistics. The PageRank scoring system was correct 53.3% of the time, while the eigen centrality scoring was slightly better at 56.6%.

## Discussion
Looking at the results, there were some significant omissions such as Lebron James, Kevin Durant, Stephen Curry, and other notable star players. This is due to the network favouring players that were involved in more lineups and playing more games, as they lead to more links. This can be affected by factors such as injuries and mid-season trades, where for example, Durant was traded mid-season and was injured for almost half the season, so he was barely included in any of the Suns or Nets lineups when comparing with Nic Claxton.

This model also favoured centers and interior players due to the metrics selected, since they tend to statistically perform better field goal percentage and rebounding percentages due to their role on the team. Furthermore, a player’s defensive impact was limited to opponent miss percentage and steals in this model. These issues can be addressed through selecting more metrics in general, or more advanced and specific ones. However, results could be affected by issues such as multicollinearity if they aren’t selected carefully. Computational time should also be considered.

In addition, comparing node centrality metrics overall is a slight misrepresentation of the problem space, as each metric is specific to each team. The best player on a well-rounded team would have a lower centrality score compared to the best player on a team that is less balanced, as the player in the first case would be surrounded by nodes with higher importance score, dampening their overall score. 

This model is, however, able to effectively rate players based on their availability to the team. This was demonstrated in the results, where players such as Kevon Looney and Ivica Zubac, who played 82 and 76 games respectively, were selected as the most important players on their team over the star players.

## Conclusion 

This project was able to apply network theory concepts to analyse player performance but performed mediocre when comparing with existing advanced statistics. Games played and efficiency was discovered as the main contributing factor for the model outputs. 

Further research could involve advancements in the edge weighing function, specifically the adjustment multipliers, and incorporate more advanced offensive and defensive metrics such as effective field goal percentage, usage rate and box plus minus to provide more accurate results. In addition, play-by-play data could be experimented with, in order to provide a more specific evaluation for how each player performed in lineup. 

