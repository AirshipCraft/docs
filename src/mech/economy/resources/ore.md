# Ores
By combining the Node's system of spawning ores with a bias algorithm, we can create a more realistic and immersive gameplay experience, as certain ores would be more abundant in certain regions and rare in others.

## Overview
The HiddenOre system will also add an extra layer of excitement and challenge to the game, as players will have to explore and search for ores in hidden locations, rather than relying on the traditional method of mining. It will encourage players to be more adventurous and explorative, which will add more depth to the gameplay.
   
Overall, this hybrid system will make the economy more interesting and dynamic, and create a more realistic and immersive gameplay experience.
   
To start, let's discuss the current system used in phonon's [nodes](https://github.com/phonon/minecraft-nodes) plugin. In this system, ores are spawned based on the type of node (such as a forest node or a mountain node) and the depth of the block being generated. This means that certain ores will only spawn in certain types of nodes, and at certain depths. However, this system can lead to a lack of scarcity in resources, as players can easily find all the ores they need within their own node.
   
To create a more realistic and challenging resource management system, we can add an algorithm that creates latitudinal and longitudinal biases in ore generation. This means that certain types of ores will be more common in certain areas of the map, and less common in others.
   
Here's how this could work in practice:
   
1. Determine the dimensions of the map: We need to know the size of the map in order to properly generate ore distribution.
2. Divide the map into sections: We can divide the map into sections based on latitudinal and longitudinal lines. For example, we could divide the map into a grid of 100x100 chunks. Each chunk would have its own ore distribution based on the algorithm we create. Fortunately enough, since we are using the Paralon map this part is already done for us.

![climate_map](/src/images/climate_map.webp)

3. Generate ore biases: We can create an algorithm that generates biases for each type of ore in each chunk. These biases could be based on factors such as the distance from the equator or the distance from the center of the map. We could also take into account the type of biome or node present in each chunk.

```java
for each chunk in the map:
    for each ore type:
        lat_bias = function(chunk.latitude)
        long_bias = function(chunk.longitude)
        biome_bias = function(chunk.biome)
        depth_bias = function(block.depth)
        total_bias = lat_bias * long_bias * biome_bias * depth_bias
        set ore bias in chunk for ore type to total_bias
```

4. Generate ore deposits: Using the biases we generated, we can then generate ore deposits within each chunk. We can use a system similar to Programmerdan's HiddenOre plugin to generate ore deposits that are hidden until a player uncovers them.

```java
for each chunk in the map:
    for each ore type:
        ore_bias = get ore bias in chunk for ore type
        for each block in the chunk:
            if block is stone and random value < ore_bias:
                generate ore deposit centered around block
                hide ore deposit until player uncovers it
```

5. Adjust ore respawn rates: To prevent players from simply moving from chunk to chunk to find all the ore they need, we can adjust the respawn rates of ore deposits based on the number of times they've been mined. For example, an ore deposit that has been mined 10 times might have a much slower respawn rate than a deposit that has only been mined once.

```java
on ore deposit mined:
    ore_deposit.times_mined++
    ore_deposit.respawn_rate = base_respawn_rate * 1.5^ore_deposit.times_mined
```
   
With this system, players will need to explore different areas of the map in order to find all the resources they need. This adds an element of challenge and realism to the game, and encourages players to interact with each other to trade resources and form alliances.

## Effect on Gameplay
The scarcity of resources will have a significant impact on the economy of the server. With ores being harder to find, players will have to work harder to obtain them, and this could result in an increase in the value of the ores in the market. This could lead to a more dynamic and complex economy, where the value of different ores fluctuates based on their scarcity.
   
It is important to balance the ore generation system so that it is not too difficult for players to find ores, but also not too easy. If it is too difficult, players may become frustrated and lose interest in mining altogether, and if it is too easy, it could lead to an overabundance of resources and hurt the economy. It may be useful to collect data on ore generation rates and adjust them as necessary to achieve a balance.
   
Another way to make the ore generation more interesting and challenging would be to modify the ore generation rates based on the biome. For example, ores could be more common in underground caves or mountains, but more scarce in forests or oceans. This could encourage players to explore different biomes to find the ores they need.
   
The scarcity of resources could also encourage players to work together and form communities to share resources and pool their efforts in mining. This could create a more collaborative and social environment on the server, where players are more likely to work together and help each other out.
    
Resource scarcity could also make gameplay more challenging and rewarding. Players may need to be more strategic in their mining efforts, and prioritize which ores to go after based on their rarity and value. This could lead to a more dynamic and interesting gameplay experience, where players have to think carefully about their actions and make strategic decisions.