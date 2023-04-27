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

## Bias Algorithm
First, let's review what we're trying to accomplish with the bias algorithm. We want to modify the generation of ore blocks in Minecraft so that certain types of ore are more likely to generate in specific areas, while still maintaining a natural look and feel to the world. To achieve this, we'll need to analyze the terrain in each chunk and adjust the probability of different ore types generating based on certain factors, such as altitude, biome, and proximity to other types of blocks.
   
Here's a step-by-step breakdown of how we can implement the bias algorithm:
   
1. We'll start by defining a list of ore types that we want to bias. For example, we might want to bias the generation of diamonds, emeralds, and gold in certain areas.

2. Next, we'll need to create a method that calculates the probability of each ore type generating in a given block. This method will take in the coordinates of the block, as well as any relevant factors such as altitude, biome, and proximity to other blocks. It will output a value between 0 and 1, indicating the probability of the ore generating in that block.
    
Here's some pseudocode for this method:
```java
public static double calculateOreProbability(int x, int y, int z, BlockType oreType) {
    // calculate the base probability of the ore generating in this block
    double baseProbability = 0.0;
    if (oreType == BlockType.DIAMOND_ORE) {
        // calculate the base probability for diamond ore based on altitude
        baseProbability = calculateDiamondProbability(y);
    } else if (oreType == BlockType.EMERALD_ORE) {
        // calculate the base probability for emerald ore based on biome
        Biome biome = getBiome(x, z);
        baseProbability = calculateEmeraldProbability(biome);
    } else if (oreType == BlockType.GOLD_ORE) {
        // calculate the base probability for gold ore based on proximity to other blocks
        baseProbability = calculateGoldProbability(x, y, z);
    }
    
    // apply any additional biases based on the surrounding blocks
    double blockBias = calculateBlockBias(x, y, z, oreType);
    double totalProbability = baseProbability * blockBias;
    
    return totalProbability;
}
```
In this pseudocode, `'calculateDiamondProbability'` would be a method that calculates the base probability of diamond ore generating in a block based on its Y coordinate, `'calculateEmeraldProbability'` would calculate the probability of emerald ore generating based on the biome, and `'calculateGoldProbability'` would calculate the probability of gold ore generating based on the proximity to other blocks.
    
3. Once we have the probability of each ore type generating in each block, we'll need to modify the Minecraft world generation algorithm to take these probabilities into account. One way to do this would be to hook into the `ChunkGenerator` class provided by the Spigot or Paper API, which is responsible for generating the terrain for each chunk.

The `ChunkGenerator` is responsible for generating the blocks in a chunk. In order to implement a bias algorithm for ore generation, we would need to create a custom `ChunkGenerator` that modifies the standard ore generation to bias the distribution of ores.

Here's a general outline of the steps we would need to take:

1. Create a new class that implements the ChunkGenerator interface. This will allow us to generate custom chunks.

2. Override the generateChunkData method. This is where we will modify the standard ore generation to implement our bias algorithm.

3. In the generateChunkData method, we will first call the standard ore generation code to generate the standard ore veins. We will then modify the ore distribution based on our bias algorithm.

4. To implement the bias algorithm, we will use a noise function to generate a value for each block in the chunk. We will then use this value to adjust the number of ore blocks that are placed in each chunk. The exact details of how we implement the noise function will depend on the specific bias algorithm we choose.

5. Once we have adjusted the ore distribution, we can generate the rest of the blocks in the chunk as usual.

Here's some pseudocode to give you an idea of what the implementation might look like:
```java
public class CustomChunkGenerator implements ChunkGenerator {
    // Standard ore generation settings
    private final OreSettings oreSettings = new OreSettings();

    @Override
    public ChunkData generateChunkData(World world, Random random, int chunkX, int chunkZ, BiomeGrid biome) {
        ChunkData chunkData = createChunkData(world);

        // Generate standard ore veins
        oreSettings.generate(chunkData, random, chunkX, chunkZ);

        // Generate noise function for ore distribution bias
        NoiseGenerator noiseGenerator = new PerlinNoiseGenerator(random.nextLong(), 3, 0.5, 0.5);

        // Adjust ore distribution based on noise function
        for (int x = 0; x < 16; x++) {
            for (int y = 0; y < 256; y++) {
                for (int z = 0; z < 16; z++) {
                    double noiseValue = noiseGenerator.getValue((chunkX * 16 + x) / 16.0, y / 256.0, (chunkZ * 16 + z) / 16.0);
                    if (noiseValue > 0) {
                        // Increase ore distribution
                        int count = chunkData.getBlockTypeId(x, y, z) == Material.STONE.getId() ? 2 : 0;
                        chunkData.setBlock(x, y, z, Material.DIAMOND_ORE);
                        for (int i = 0; i < count; i++) {
                            int offsetX = random.nextInt(3) - 1;
                            int offsetY = random.nextInt(3) - 1;
                            int offsetZ = random.nextInt(3) - 1;
                            int newX = x + offsetX;
                            int newY = y + offsetY;
                            int newZ = z + offsetZ;
                            if (chunkData.getBlockTypeId(newX, newY, newZ) == Material.STONE.getId()) {
                                chunkData.setBlock(newX, newY, newZ, Material.DIAMOND_ORE);
                            }
                        }
                    } else if (noiseValue < 0) {
                        // Decrease ore distribution
                        if (chunkData.getBlockTypeId(x, y, z) == Material.DIAMOND_ORE.getId()) {
                            chunkData.setBlock(x, y, z, Material.STONE);
                        }
                    }
                }
            }
        }
    }
}
```
    
Here's some pseudocode for a possible implementation of the bias algorithm:
    
```java
// Define the parameters for generating biases
double latRange = mapDimensions.latMax - mapDimensions.latMin;
double longRange = mapDimensions.longMax - mapDimensions.longMin;
double centerLat = (mapDimensions.latMax + mapDimensions.latMin) / 2.0;
double centerLong = (mapDimensions.longMax + mapDimensions.longMin) / 2.0;
double maxBias = 2.0; // This is the maximum possible bias value for an ore type

// Loop through each chunk on the map
for (Chunk chunk : map.getChunks()) {
    // Calculate the latitudinal and longitudinal biases for this chunk
    double latBias = Math.sin((chunk.getCenter().getLatitude() - centerLat) * Math.PI / latRange);
    double longBias = Math.sin((chunk.getCenter().getLongitude() - centerLong) * Math.PI / longRange);

    // Calculate the biome bias for this chunk
    Biome biome = chunk.getBiome();
    double biomeBias = getBiomeBias(biome);

    // Loop through each ore type and calculate its total bias for this chunk
    for (OreType oreType : OreType.values()) {
        double depthBias = getDepthBias(chunk.getDepth(), oreType);
        double totalBias = latBias * longBias * biomeBias * depthBias * maxBias;
        chunk.setOreBias(oreType, totalBias);
    }
}
```
   
This pseudocode calculates the latitudinal and longitudinal biases for each chunk on the map, based on their distance from the center of the map. It then calculates the biome bias for each chunk, based on the type of biome it's in, and the depth bias for each ore type at that depth.
   
Finally, it calculates the total bias for each ore type in each chunk by multiplying these biases together, and sets the ore bias value for that ore type in that chunk. This value will be used later when generating ore deposits within the chunk.
   
To elaborate, the pseudocode sets up the parameters for generating biases. `latRange` and `longRange` represent the difference between the maximum and minimum latitude and longitude values for the map, while `centerLat` and `centerLong` represent the latitude and longitude of the center of the map. These values are used to calculate the latitudinal and longitudinal biases for each chunk, based on their distance from the center of the map.
   
The `maxBias` value represents the maximum possible bias value for an ore type. This value is used to ensure that biases are normalized and fall within a specific range.

Next, the pseudocode loops through each chunk on the map and calculates its latitudinal and longitudinal biases using the formula: `Math.sin((chunk.getCenter().getLatitude() - centerLat) * Math.PI / latRange)` and `Math.sin((chunk.getCenter().getLongitude() - centerLong) * Math.PI / longRange)`. These values are used to reflect the position of the chunk relative to the center of the map.

The pseudocode then calculates the biome bias for each chunk using the `getBiomeBias` function, which takes the chunk's biome as input and returns a bias value. Biome biases allow us to generate different types of ores in different biomes, depending on the environmental conditions and resources available.

After calculating the biome bias, the pseudocode loops through each ore type and calculates its total bias for this chunk. The depth bias for each ore type is determined using the `getDepthBias` function, which takes the depth of the chunk and the ore type as inputs and returns a bias value. Multiplying the latitudinal, longitudinal, biome, depth, and maximum biases together yields the total bias value for that ore type in that chunk.

Finally, the pseudocode sets the ore bias value for that ore type in that chunk using the `chunk.setOreBias` method. These ore biases will be used later when generating ore deposits within the chunk. By using bias algorithms, the ore generation process is made more dynamic and realistic, and can be customized to suit the specific needs of the game.
   
Here's an example implementation of a custom chunk generator that incorporates the ore bias algorithm:

```java
public class CustomChunkGenerator extends ChunkGenerator {
    private final MapDimensions mapDimensions;
    private final double latRange;
    private final double longRange;
    private final double centerLat;
    private final double centerLong;
    private final double maxBias = 2.0;

    public CustomChunkGenerator(MapDimensions mapDimensions) {
        this.mapDimensions = mapDimensions;
        this.latRange = mapDimensions.latMax - mapDimensions.latMin;
        this.longRange = mapDimensions.longMax - mapDimensions.longMin;
        this.centerLat = (mapDimensions.latMax + mapDimensions.latMin) / 2.0;
        this.centerLong = (mapDimensions.longMax + mapDimensions.longMin) / 2.0;
    }

    @Override
    public ChunkData generateChunkData(World world, Random random, int chunkX, int chunkZ, BiomeGrid biome) {
        ChunkData chunkData = createChunkData(world);

        // Calculate the latitudinal and longitudinal biases for this chunk
        double latBias = Math.sin((chunkX * 16 + 8 - centerLat) * Math.PI / latRange);
        double longBias = Math.sin((chunkZ * 16 + 8 - centerLong) * Math.PI / longRange);

        // Loop through each ore type and calculate its total bias for this chunk
        for (OreType oreType : OreType.values()) {
            double depthBias = getDepthBias(chunkData.getMinHeight(), oreType);
            double biomeBias = getBiomeBias(biome.getBiome(8, 8));
            double totalBias = latBias * longBias * biomeBias * depthBias * maxBias;

            // Set the ore bias for this ore type in this chunk
            chunkData.setRegion(
                0, oreType.ordinal(), 0,
                16, oreType.getMaxHeight() - oreType.getMinHeight() + 1, 16,
                new MaterialData(oreType.getMaterial()),
                new double[16][oreType.getMaxHeight() - oreType.getMinHeight() + 1][16]
            );
            chunkData.setRegion(
                0, oreType.ordinal(), 0,
                16, oreType.getMaxHeight() - oreType.getMinHeight() + 1, 16,
                null,
                new double[16][oreType.getMaxHeight() - oreType.getMinHeight() + 1][16]
            );
            chunkData.setRegion(
                0, oreType.ordinal(), 0,
                16, oreType.getMaxHeight() - oreType.getMinHeight() + 1, 16,
                null,
                new double[16][oreType.getMaxHeight() - oreType.getMinHeight() + 1][16]
            );
            chunkData.setRegion(
                0, oreType.ordinal(), 0,
                16, oreType.getMaxHeight() - oreType.getMinHeight() + 1, 16,
                null,
                new double[16][oreType.getMaxHeight() - oreType.getMinHeight() + 1][16]
            );

            // Set the ore bias for this ore type in this chunk
            for (int x = 0; x < 16; x++) {
                for (int z = 0; z < 16; z++) {
                    for (int y = oreType.getMinHeight(); y <= oreType.getMaxHeight(); y++) {
                        chunkData.setBlock(
                            x, y, z,
                            oreType.getMaterial(),
                            new double[]{totalBias}
                        );
                    }
                }
            }
        }

        return chunkData
    }
}
```