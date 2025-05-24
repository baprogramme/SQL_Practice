# 🍕 3-Topping Pizza Cost Analysis

You’re a consultant for a major pizza chain that will be running a promotion where all **3-topping pizzas** will be sold for a fixed price, and are trying to understand the **costs involved**.

Given a list of pizza toppings, consider **all the possible 3-topping pizzas**, and print out the **total cost** of those 3 toppings. Sort the results with the **highest total cost on the top**, followed by **pizza toppings in ascending order**.

Break ties by listing the ingredients in **alphabetical order**, starting from the **first ingredient**, followed by the **second and third**.

> **P.S.** Be careful with the **spacing (or lack of)** between each ingredient. Refer to our Example Output.

---

## 📝 Notes

- Do **not display pizzas** where a topping is repeated.  
  ❌ `Pepperoni,Pepperoni,Onion Pizza`  
- Ingredients must be listed in **alphabetical order**.  
  ✅ `Chicken,Onions,Sausage`  
  ❌ `Onion,Sausage,Chicken`

---

## 📦 Table: `pizza_toppings`

| Column Name     | Type           |
|-----------------|----------------|
| `topping_name`  | varchar(255)   |
| `ingredient_cost` | decimal(10,2) |

---

## 📥 Example Input

| topping_name   | ingredient_cost |
|----------------|-----------------|
| Pepperoni      | 0.50            |
| Sausage        | 0.70            |
| Chicken        | 0.55            |
| Extra Cheese   | 0.40            |

---

## 📤 Example Output

| pizza                           | total_cost |
|---------------------------------|------------|
| Chicken,Pepperoni,Sausage       | 1.75       |
| Chicken,Extra Cheese,Sausage    | 1.65       |
| Extra Cheese,Pepperoni,Sausage  | 1.60       |
| Chicken,Extra Cheese,Pepperoni  | 1.45       |

---

## 💡 Explanation

There are four different combinations of the three toppings.  
Cost of the pizza with toppings `Chicken, Pepperoni and Sausage` is:

$0.55 (Chicken) + $0.50 (Pepperoni) + $0.70 (Sausage) = $1.75


Additionally, they are arranged alphabetically. In the dictionary, **Chicken** comes before **Pepperoni**, and **Pepperoni** comes before **Sausage**.

> 🧠 **Note:**  
> The dataset you are querying against may have different input & output — this is just an example!

``` sql

SELECT 
    CONCAT(t1.topping_name, ',', t2.topping_name, ',', t3.topping_name) AS pizza,
    ROUND(t1.ingredient_cost + t2.ingredient_cost + t3.ingredient_cost, 2) AS total_cost
FROM 
    pizza_toppings t1
JOIN 
    pizza_toppings t2 ON t1.topping_name < t2.topping_name
JOIN 
    pizza_toppings t3 ON t2.topping_name < t3.topping_name
ORDER BY 
    total_cost DESC,
    pizza ASC;
