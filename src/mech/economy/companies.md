> <span style="color:red">Currently unimplemented.</span>
# Companies

## Overview
By introducing the concept of companies in our economy system, we can create a more diverse and dynamic in-game economy. Players can invest in companies and share in their profits, while companies can expand their reach and profitability by owning and operating in-game shops. We believe this mechanic will add a new level of depth and engagement to our server.

## Company Registration

To register a company, a player must have a certain amount of money in their account. We can set a minimum amount required to register a company, such as 100,000 in-game currency. Once the player has the required amount, they can run the following command to register a company:

```
/company register [company name]
```

This command will create a new company with the specified name and set the player as the owner. The company's information will be stored in a separate database table from the player's information.

## Company Stocks

Players can purchase stocks in registered companies using the following command:

```
/company buy [company name] [amount]
```

The amount parameter is the number of stocks the player wants to purchase. The price of each stock can be set by the company owner and can vary depending on the company's performance.

Players can also sell their stocks back to the company using the following command:

```
/company sell [company name] [amount]
```

The amount parameter is the number of stocks the player wants to sell. The price of each stock can be set by the company owner and can vary depending on the company's performance.

## Company Owned Shops and Profit Sharing

Companies can own and profit from in-game shops by purchasing them using the main market plugin. The company owner can run the following command to purchase a shop:

```
/shop buy [shop ID] [company name]
```

The shop ID parameter is the ID of the shop the company wants to purchase. The company name parameter is the name of the company making the purchase.

Once the shop is owned by the company, any profits made from the shop will be added to the company's account. The company owner can view the shop's profits using the following command:

```
/shop profits [shop ID]
```

## Technical Overview

To begin with, we'll need to create a new entity class called `Company`. This class will have attributes such as `name`, `owner`, `stockPrice`, `totalStock`, `sharesSold`, `shops`, and `profits`. The `shops` attribute will be a list of all the shops owned by the company, and the `profits` attribute will be a map that associates each shop with its current profit.

```java
public class Company {
    private String name;
    private UUID owner;
    private double stockPrice;
    private int totalStock;
    private int sharesSold;
    private List<Shop> shops;
    private Map<Shop, Double> profits;
    
    // constructor, getters, and setters
}
```

Next, we'll need to create methods for registering a new company, purchasing stock in a company, and sharing profits from company-owned shops. 

```java
public class CompanyManager {
    private List<Company> companies = new ArrayList<>();

    public void registerCompany(String name, UUID owner, double stockPrice, int totalStock) {
        Company company = new Company(name, owner, stockPrice, totalStock, new ArrayList<>(), new HashMap<>());
        companies.add(company);
    }
    
    public void purchaseStock(Player player, Company company, int numShares) {
        double cost = numShares * company.getStockPrice();
        if (player.getBalance() >= cost && company.getSharesSold() + numShares <= company.getTotalStock()) {
            player.withdraw(cost);
            company.setSharesSold(company.getSharesSold() + numShares);
            double ownership = numShares / (double) company.getTotalStock();
            company.getOwners().put(player.getUniqueId(), ownership);
        }
    }
    
    public void shareProfits() {
        for (Company company : companies) {
            for (Shop shop : company.getShops()) {
                double revenue = shop.getRevenue();
                double profit = revenue - shop.getCost();
                company.getProfits().put(shop, profit);
            }
            double totalProfit = company.getProfits().values().stream().mapToDouble(Double::doubleValue).sum();
            for (Map.Entry<UUID, Double> entry : company.getOwners().entrySet()) {
                Player player = Bukkit.getPlayer(entry.getKey());
                if (player != null) {
                    double share = entry.getValue() * totalProfit;
                    player.deposit(share);
                }
            }
        }
    }
}
```

Finally, we'll need to hook into the main [market](/src/mech/economy/market.md) plugin to make sure that company-owned shops are treated the same way as player-made shops. We can do this by adding a listener that listens for when a shop is created, and then adding that shop to the corresponding company's `shops` list.

```java
public class ShopListener implements Listener {
    private EconomySystem economySystem;

    public ShopListener(EconomySystem economySystem) {
        this.economySystem = economySystem;
    }

    @EventHandler
    public void onShopCreate(ShopCreateEvent event) {
        Player player = event.getPlayer();
        Shop shop = event.getShop();
        for (Company company : economySystem.getCompanies()) {
            if (company.getOwner().equals(player.getUniqueId())) {
                company.getShops().add(shop);
            }
        }
    }
}
```

And that's it! With these changes, players can now register companies, purchase stock in those companies, and share profits from company-owned shops.