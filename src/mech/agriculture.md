# Agriculture
"Agriculture" is a placeholder name for the catch-all system to handle horticulture, agricultural, botanical, and zoological features.

## Overview
This system is all encompassing and handles a lot of different things, all features and changes are mainly concerning farming and farming related things.
    
We plan to introduce several new types of farming equipment, including fish traps, animal traps, and other tools that can be used for hunting and farming. These tools will allow players to experiment with new farming methods and hopefully add some variety to the gameplay.
   
We also plan to introduce a variety of new crops and crop types that players can grow, including exotic fruits and vegetables, herbs, and spices. We also plan to add new mechanics for growing and harvesting crops, such as custom plants and trees that players can grow, with unique properties and characteristics that are not found in the vanilla game. These plants and trees will provide new resources and crafting materials, and add to the overall variety of the game.
   
We want to make changes to the animal breeding system, to make it more realistic and immersive. This will include changes to the way animals behave, as well as new mechanics for breeding and raising animals. This way animals will naturally increase in population in the wilderness and new population caps for animals will be introduced to prevent overbreeding or perhaps introducing the extinction of an animal from an area.

## New Items
In this section we will cover the new items planned to be added as part of this plugin. 

### Fish Traps
Fish traps are a type of equipment that can be used to passively collect fish from bodies of water. They look like barrels and can be placed in a minimum of a 3x3 pool of water. The fish traps have a custom crafting recipe that looks like this:
   
[Placeholder image of crafting recipe]()
   
To use the traps, players must first craft them and then place them in a body of water. Once the trap is in place, players must put food in the trap to act as bait. The type of bait used will determine the type of fish that can be caught. For example, using raw salmon as bait will attract salmon, while using raw cod will attract cod.
   
The traps will collect fish over time, and the amount and type of fish caught will depend on a variety of factors, such as the type of bait used, the size of the pool of water, and the length of time the trap has been in place. Once the trap has caught fish, players can come back to collect them at any time.
   
Fish traps can be a useful addition to a player's farming equipment, providing a passive way to collect fish and adding a new dimension to gameplay.

#### ***Technical Perspective***
We can design a class called `FishTrap` to use with the plugin. 
    
The `FishTrap` class is a custom object that represents a fish trap in the game. It has the following fields:
    
* `location` : The location where the trap is placed.
* `bait` : The amount of bait in the trap.
* `timeUntilCheck` : The time remaining until the trap can be checked again.
    
The class has a constructor that takes in a `'Location'` object, which represents the location where the trap will be placed. The constructor also initializes the `'bait'` and `'timeUntilCheck'` fields to their default values.
   
The `addBait()` method allows players to add bait to the trap. If the amount of bait in the trap exceeds the maximum amount allowed, the excess bait is not added and the method returns `false`.
   
The `checkTrap()` method is called when a player wants to check the trap for fish. If there is no bait in the trap, the method returns `false`, indicating that no fish were caught. If there is bait in the trap, the amount of bait is decreased and a random number is generated to determine if a fish was caught. If a fish was caught, the method returns `true`, indicating that the player caught a fish. If no fish was caught, the method returns `false`.
   
The `getTimeUntilCheck()` method returns the time remaining until the trap can be checked again.
   
The `decreaseTimeUntilCheck()` method decreases the time remaining until the trap can be checked again.
   
The `getLocation()` method returns the location of the trap.
   
The `equals()` and `hashCode()` methods are overridden to allow the traps to be compared and hashed properly.
   
Overall, the FishTrap class allows players to create and use fish traps in the game, and provides methods for adding bait, checking the trap for fish, and managing the time until the trap can be checked again.

```java
import org.bukkit.Location;
import java.util.Objects;
import java.util.Random;

public class FishTrap {
    private final Location location;
    private int bait;
    private long timeUntilCheck;

    // Constructor
    public FishTrap(Location location) {
        this.location = location;
        this.bait = 0;
        this.timeUntilCheck = 0;
    }

    // Add bait to the trap
    public boolean addBait(int amount) {
        int maxBait = 64;
        if (this.bait + amount > maxBait) {
            return false;
        }
        this.bait += amount;
        return true;
    }

    // Check the trap for fish
    public boolean checkTrap() {
        if (this.bait <= 0) {
            return false;
        }
        Random rand = new Random();
        double catchChance = 0.5; // Adjust this value to change catch rate
        if (rand.nextDouble() < catchChance) {
            this.bait--;
            return true;
        }
        this.bait--;
        return false;
    }

    // Get the time until the trap can be checked again
    public long getTimeUntilCheck() {
        return this.timeUntilCheck;
    }

    // Decrease the time until the trap can be checked again
    public void decreaseTimeUntilCheck(long amount) {
        this.timeUntilCheck = Math.max(this.timeUntilCheck - amount, 0);
    }

    // Get the location of the trap
    public Location getLocation() {
        return this.location;
    }

    // Override equals() method to compare FishTrap objects
    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (!(o instanceof FishTrap)) {
            return false;
        }
        FishTrap fishTrap = (FishTrap) o;
        return Objects.equals(this.location, fishTrap.location);
    }

    // Override hashCode() method to hash FishTrap objects
    @Override
    public int hashCode() {
        return Objects.hash(this.location);
    }
}
```
   
Here is an example listener to handle interactions with the fish trap:
   
```java
// Handle player interactions with fish traps
    @EventHandler
    public void onPlayerInteract(PlayerInteractEvent event) {
        Player player = event.getPlayer();
        ItemStack item = player.getInventory().getItemInMainHand();
        Block clickedBlock = event.getClickedBlock();

        // Check if player right-clicked a fish trap with bait
        if (event.getAction() == Action.RIGHT_CLICK_BLOCK && clickedBlock.getType() == Material.BARREL) {
            FishTrap trap = plugin.getFishTrapManager().getFishTrap(clickedBlock.getLocation());
            if (trap != null && trap.checkTrap()) {
                player.sendMessage(ChatColor.GREEN + "You caught a fish!");
            } else {
                player.sendMessage(ChatColor.RED + "No fish were caught.");
            }
        }

        // Check if player is placing a fish trap with bait
        if (event.getAction() == Action.RIGHT_CLICK_BLOCK && item.getType() == Material.BARREL) {
            Location location = clickedBlock.getLocation().add(event.getBlockFace().getDirection());
            FishTrap trap = new FishTrap(location);
            if (trap.addBait(1)) {
                plugin.getFishTrapManager().addFishTrap(trap);
                player.sendMessage(ChatColor.GREEN + "You placed a fish trap!");
            } else {
                player.sendMessage(ChatColor.RED + "Fish trap is already full!");
            }
        }
    }
```

