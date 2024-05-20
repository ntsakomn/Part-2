using System;
using System.Collections.Generic;
using System.Linq;

namespace RecipeApp
{
    class Program
    {
        static void Main(string[] args)
        {
            // Initialize a list to store recipes
            List<Recipe> recipes = new List<Recipe>();
            bool exit = false;

            // Main loop to interact with the user
            while (!exit)
            {
                // Display menu options
                Console.WriteLine("1. Enter Recipe Details");
                Console.WriteLine("2. Display Recipes");
                Console.WriteLine("3. Select Recipe");
                Console.WriteLine("4. Exit");
                Console.Write("Select an option: ");

                // Read user input and execute corresponding action
                switch (Console.ReadLine())
                {
                    case "1":
                        EnterRecipeDetails(recipes);
                        break;
                    case "2":
                        DisplayRecipes(recipes);
                        break;
                    case "3":
                        SelectRecipe(recipes);
                        break;
                    case "4":
                        exit = true;
                        break;
                    default:
                        Console.WriteLine("Invalid option. Please try again.");
                        break;
                }
            }
        }

        // Method to enter recipe details
        static void EnterRecipeDetails(List<Recipe> recipes)
        {
            Recipe recipe = new Recipe();

            Console.Write("Enter recipe name: ");
            recipe.Name = Console.ReadLine();

            Console.Write("Enter the number of ingredients: ");
            int ingredientCount = Convert.ToInt32(Console.ReadLine());

            // Loop to input each ingredient
            for (int i = 0; i < ingredientCount; i++)
            {
                Console.Write($"Enter the name of ingredient {i + 1}: ");
                string name = Console.ReadLine();

                Console.Write($"Enter the quantity of {name}: ");
                double quantity = Convert.ToDouble(Console.ReadLine());

                Console.Write($"Enter the unit of measurement for {name}: ");
                string unit = Console.ReadLine();

                Console.Write($"Enter the number of calories for {name}: ");
                int calories = Convert.ToInt32(Console.ReadLine());

                Console.Write($"Enter the food group for {name}: ");
                string foodGroup = Console.ReadLine();

                recipe.AddIngredient(new Ingredient(name, quantity, unit, calories, foodGroup));
            }

            // Add the recipe to the list
            recipes.Add(recipe);

            Console.WriteLine("Recipe details entered successfully.");
        }

        // Method to display all recipes
        static void DisplayRecipes(List<Recipe> recipes)
        {
            Console.WriteLine("Recipes:");
            foreach (var recipe in recipes.OrderBy(r => r.Name))
            {
                Console.WriteLine(recipe.Name);
            }
        }

        // Method to select and display a recipe
        static void SelectRecipe(List<Recipe> recipes)
        {
            Console.WriteLine("Select a recipe:");

            // Display all recipes
            DisplayRecipes(recipes);

            Console.Write("Enter recipe name: ");
            string recipeName = Console.ReadLine();

            // Find the selected recipe
            Recipe selectedRecipe = recipes.FirstOrDefault(r => r.Name.Equals(recipeName, StringComparison.OrdinalIgnoreCase));

            if (selectedRecipe != null)
            {
                // Display the selected recipe details
                selectedRecipe.Display();
            }
            else
            {
                Console.WriteLine("Recipe not found.");
            }
        }
    }

    // Represents a recipe
    class Recipe
    {
        public string Name { get; set; }
        private List<Ingredient> ingredients;

        // Constructor
        public Recipe()
        {
            ingredients = new List<Ingredient>();
        }

        // Method to add an ingredient to the recipe
        public void AddIngredient(Ingredient ingredient)
        {
            ingredients.Add(ingredient);
        }

        // Method to display the recipe details
        public void Display()
        {
            Console.WriteLine($"Recipe: {Name}");
            Console.WriteLine("Ingredients:");
            foreach (var ingredient in ingredients)
            {
                Console.WriteLine($"{ingredient.Quantity} {ingredient.Unit} of {ingredient.Name}");
            }

            // Calculate and display total calories
            int totalCalories = ingredients.Sum(i => i.Calories);
            Console.WriteLine($"Total Calories: {totalCalories}");

            // Notify if total calories exceed 300
            if (totalCalories > 300)
            {
                Console.WriteLine("Warning: Total calories exceed 300.");
            }
        }
    }

    // Represents an ingredient
    class Ingredient
    {
        public string Name { get; }
        public double Quantity { get; }
        public string Unit { get; }
        public int Calories { get; }
        public string FoodGroup { get; }

        // Constructor
        public Ingredient(string name, double quantity, string unit, int calories, string foodGroup)
        {
            Name = name;
            Quantity = quantity;
            Unit = unit;
            Calories = calories;
            FoodGroup = foodGroup;
        }
    }
}
