# 003 - yaml (Yet Another MarkUp Language)

## dictionary vs list vs list of dictionaries

```yaml
Color: Blue
Model:
	Name: Corvette
	Model: 1995
Transition: manual
Price: $20,000
```

### common data types

#### key value pair

```yaml
Fruit: Apple
Vegetable: Carrot
Liquid: Water
Meat: Chicken
```

#### array/list

```yaml
Fruits:
- Orange
- Apple
- Banana

Vegetables:
- Carrot
- Cucumber
```

#### dictionary/map

```yaml
Banana:
	Calories: 105
	Fat: 0.4g
	Carbs: 27g

Grapes:
	Calories: 62
	Fat: 0.3g
	Carbs: 16g
```

#### yaml advanced

```yaml
Fruits:
	- Banana:
		Calories: 105
		Fat: 0.4g
		Carbs: 27g

	- Grapes:
		Calories: 62
		Fat: 0.3g
		Carbs: 16g
```

#### notes

* Dictionary - Unordered && List            -  Ordered
* Hash (#) - comment