# ======= MINI RPG SHOP =======
items = {
    "1": {"name": "Berserker Axe", "price": 39.0, "stock": 2},
    "2": {"name": "Regeneration Potion II", "price": 26.0, "stock": 6},
    "3": {"name": "Lizard Sandwich", "price": 7.0, "stock": 9},
    "4": {"name": "Treasure Map", "price": 125.0, "stock": None }  # stok tak terbatas
}

money = 999.0
inventory = {"treasure map": 1}
state = "MENU"
running = True

# alias per state
menu_aliases = {
    "1": "SHOP", "shop": "SHOP", "visit shop": "SHOP","visit": "SHOP",
    "2": "INVENTORY", "inventory": "INVENTORY", "inven": "INVENTORY",
    "check": "INVENTORY", "check inv": "INVENTORY",
    "3": "EXIT", "exit": "EXIT", "exit game": "EXIT",
}

shop_aliases = {
    "x": "MENU", "back": "MENU", "exit": "MENU", "menu": "MENU"
}

inv_aliases = {
    "": "MENU", "menu": "MENU", "back": "MENU", "x": "MENU", "exit": "MENU"
}

while running:
    if state == "MENU":
        print("\n=== MAIN MENU ===")
        print(f"Your balance: {money}")
        print("1. Visit Shop")
        print("2. Check Inventory")
        print("3. Exit Game")

        choice = input("Choose option: ").lower().strip()
        next_state = menu_aliases.get(choice)

        if next_state == "SHOP":
            state = "SHOP"
        elif next_state == "INVENTORY":
            state = "INVENTORY"
        elif next_state == "EXIT":
            print("Thanks for playing! Bye!")
            running = False
        else:
            print("Invalid choice!")

    elif state == "SHOP":
        print("\n=== WELCOME TO THE SHOP ===")
        for key, item in items.items():
            stok_display = "∞" if item["stock"] is None else item["stock"]
            print(f"{key}. {item['name']} (stock: {stok_display}, Rp{item['price']})")
        print("x. Back to Main Menu")

        choice = input("Choose item number or 'x': ").lower().strip()
        if shop_aliases.get(choice) == "MENU":
            state = "MENU"
        elif choice in items:
            item = items[choice]
            stok_display = "∞" if item["stock"] is None else item["stock"]
            print(f"You chose {item['name']} (stock: {stok_display})")
            try:
                qty = int(input("How many do you want? "))
            except ValueError:
                print("Please enter a number.")
                continue

            total = item["price"] * qty
            enough_stock = (item["stock"] is None) or (qty <= item["stock"])

            if qty <= 0:
                print("Amount must be > 0")
            elif not enough_stock:
                print("Not enough stock!")
            elif total > money:
                print("Not enough money!")
            else:
                if item["stock"] is not None:
                    item["stock"] -= qty
                money -= total
                inventory[item["name"]] = inventory.get(item["name"], 0) + qty
                print(f"Bought {qty} {item['name']}. Balance: {money}")
        else:
            print("Invalid choice!")

    elif state == "INVENTORY":
        print("\n=== YOUR INVENTORY ===")
        if inventory:
            for name, qty in inventory.items():
                print(f"{name}: {qty}")
        else:
            print("Inventory is empty")

        choice = input("Press Enter or type 'menu' to return: ").lower().strip()
        if inv_aliases.get(choice) == "MENU":
            state = "MENU"
        else:
            print("Invalid input, returning to main menu…")
            state = "MENU"
