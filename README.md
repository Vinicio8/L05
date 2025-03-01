# L05
Code challenge number 5
#include < iostream >
#include < map >
#include < set >
#include < queue >
#include < stack >
#include < vector >

void displayInventory(const std::map<std::string, std::pair<int, std::string>>& inventory) {
    std::cout << "Current Inventory:\n";
    for (const auto& item : inventory) {
        std::cout << item.first << " (" << item.second.second << "): " 
                  << item.second.first << " in stock\n";
    }
    std::cout << std::endl;
}

void displayCategories(const std::set<std::string>& categories) {
    std::cout << "Product Categories:\n";
    for (const auto& category : categories) {
        std::cout << "- " << category << "\n";
    }
    std::cout << std::endl;
}

void processOrders(std::queue<std::string>& orders, std::map<std::string, std::pair<int, std::string>>& inventory) {
    std::cout << "Processing Orders:\n";
    while (!orders.empty()) {
        std::string order = orders.front();
        std::string product = order.substr(order.find(": ") + 2); 
        
        if (inventory.count(product) && inventory[product].first > 0) {
            std::cout << order << " - Completed\n";
            inventory[product].first--; 
        } else {
            std::cout << order << " - Out of Stock!\n";
        }
        orders.pop();
    }
    std::cout << std::endl;
}

void processRestocks(std::stack<std::pair<std::string, int>>& restocks, std::map<std::string, std::pair<int, std::string>>& inventory) {
    std::cout << "Processing Restocks:\n";
    while (!restocks.empty()) {
        auto item = restocks.top();
        if (inventory.count(item.first)) {
            inventory[item.first].first += item.second; 
        } else {
            inventory[item.first] = {item.second, "Uncategorized"}; 
        }
        std::cout << "Restocked " << item.second << " units of " << item.first << std::endl;
        restocks.pop();
    }
    std::cout << std::endl;
}

void displayCustomerCart(const std::vector<std::string>& cart) {
    std::cout << "Items in customer cart:\n";
    for (const auto& item : cart) {
        std::cout << "- " << item << "\n";
    }
    std::cout << std::endl;
}

void checkout(std::vector<std::string>& cart, std::queue<std::string>& orders) {
    std::cout << "Checking out customer cart...\n";
    for (const auto& item : cart) {
        orders.push("Order: " + item);
    }
    cart.clear(); 
    std::cout << "Checkout complete!\n\n";
}

int main() {
    std::map<std::string, std::pair<int, std::string>> inventory;

    std::set<std::string> productCategories;

    std::queue<std::string> orders;

    std::stack<std::pair<std::string, int>> restocks;

    std::vector<std::string> customerCart;

    productCategories.insert("Electronics");
    productCategories.insert("Accessories");
    productCategories.insert("Peripherals");

    inventory["Laptop"] = {5, "Electronics"};
    inventory["Mouse"] = {20, "Accessories"};
    inventory["Keyboard"] = {10, "Peripherals"};

    customerCart.push_back("Laptop");
    customerCart.push_back("Mouse");
    customerCart.push_back("Keyboard");

    displayCategories(productCategories);
    displayInventory(inventory);

    displayCustomerCart(customerCart);

    checkout(customerCart, orders);

    processOrders(orders, inventory);

    restocks.push({"Mouse", 10});
    restocks.push({"Laptop", 2});
    restocks.push({"Keyboard", 5});

    processRestocks(restocks, inventory);

    displayInventory(inventory);

    return 0;
}
