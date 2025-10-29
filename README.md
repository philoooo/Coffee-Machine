<h1 align="center"> Coffee Machine </h1>

A simple Python program that simulates a coffee vending machine. Users can order espresso, latte, or cappuccino, insert coins, and get their drink if enough resources and payment are available. The program keeps track of ingredients and profits.


```
MENU = {
    "espresso": {
        "ingredients": {
            "water": 50,
            "coffee": 18,
        },
        "cost": 1.5,
    },
    "latte": {
        "ingredients": {
            "water": 200,
            "milk": 150,
            "coffee": 24,
        },
        "cost": 2.5,
    },
    "cappuccino": {
        "ingredients": {
            "water": 250,
            "milk": 100,
            "coffee": 24,
        },
        "cost": 3.0,
    }
}
profit = 0
resources = {
    "water": 300,
    "milk": 200,
    "coffee": 100,
}

def is_resource_sufficient(order_ingredients):
    """Returns True if order can be made, or false when resources are insufficient"""
    for item in order_ingredients:
        if order_ingredients[item] >= resources[item]:
            print(f"Sorry there is not enough {item}.")
            return False
    return True

def process_coins():
    """Returns the total calculated from coins inserted"""
    print("Please insert coins.")
    total = int(input("How many quarters?: ")) * 0.25
    total += int(input("How many dimes?: ")) * 0.10
    total += int(input("How many nickles?: ")) * 0.05
    total += int(input("How many pennies?: ")) * 0.01
    return total

def is_transaction_successful(money_received, drink_cost):
    """Return True when payment is accepted, or False if payment is insufficient"""
    if money_received >= drink_cost:
        change = round(money_received - drink_cost, 2)
        print(f"Here is ${change} in change.")
        global profit
        profit += drink_cost
        return True
    else:
        print("Sorry that's not enough money. Money refunded.")
        return False

def make_coffee(drink_name, order_ingredients):
    """Deduct the required ingredients from the resources."""
    for item in order_ingredients:
        resources[item] -= order_ingredients[item]
    print(f"Here is your {drink_name} ☕️")


is_on = True

while is_on:
    #loop to check user's drink input
    order = input("What would you like? (espresso/latte/cappuccino): ")

    #Secret 'off' word to turn off machine
    if order == 'off':
        is_on = False

        #Report is generated showing current resource values
    elif order == 'report':
        print(f"Water: {resources["water"]}ml")
        print(f"Milk: {resources["milk"]}ml")
        print(f"Coffee: {resources["coffee"]}g")
        print(f"Money: ${profit}")
    else:
        drink = MENU[order]
        if is_resource_sufficient(drink["ingredients"]):
            payment = process_coins()
            if is_transaction_successful(payment, drink["cost"]):
                make_coffee(order, drink["ingredients"])
```

Test case to display user experience from my editor:
```
What would you like? (espresso/latte/cappuccino): latte
Please insert coins.
How many quarters?: 12
How many dimes?: 12
How many nickles?: 1
How many pennies?: 12
Here is $1.87 in change.
Here is your latte ☕️
What would you like? (espresso/latte/cappuccino): latte
Sorry there is not enough water.
```
