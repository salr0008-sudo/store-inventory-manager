# store-inventory-manager
Terminal-based store inventory manager in C++ that tracks products, stock, sales and profit.
[program.cpp](https://github.com/user-attachments/files/33026355/program.cpp)
#include "splashkit.h"
#include "splashkit-arrays.h"
#include "utilities.h"

struct product_data
{
    string name;
    double cost_price;
    double sale_price;
    int stock;
};

struct store_data
{
    dynamic_array<product_data> products;
    int total_sales;
    double total_profits;
};

void add_product(store_data &store)
{
    product_data product;

    product.name = read_string("Enter product name: ");
    product.cost_price = read_double("Enter cost price: ");
    product.sale_price = read_double("Enter sale price: ");
    product.stock = read_integer("Enter stock amount: ");

    add(store.products, product);

    write_line("Product added.");
}

void print_store(store_data &store)
{
    write_line("===== STORE PRODUCTS =====");

    for (int i = 0; i < length(store.products); i++)
    {
        product_data product = get(store.products, i);

        write_line("Index: " + to_string(i));
        write_line("Product: " + product.name);
        write_line("  Cost Price: " + to_string(product.cost_price));
        write_line("  Sale Price: " + to_string(product.sale_price));
        write_line("  Stock: " + to_string(product.stock));
        write_line("-------------------------");
    }

    write_line("Total Sales: " + to_string(store.total_sales));
    write_line("Total Profits: " + to_string(store.total_profits));
}

void delete_product(store_data &store)
{
    int index = read_integer("Enter the index to delete: ");

    if (index >= 0 && index < length(store.products))
    {
        remove(store.products, index);
        write_line("Product deleted.");
    }
    else
    {
        write_line("Invalid index.");
    }
}

void update_product(store_data &store)
{
    int index = read_integer("Enter the index of the product to update: ");

    if (index >= 0 && index < length(store.products))
    {
        product_data product = get(store.products, index);

        product.name = read_string("Enter product name: ");
        product.cost_price = read_double("Enter cost price: ");
        product.sale_price = read_double("Enter sale price: ");
        product.stock = read_integer("Enter stock amount: ");

        set(store.products, index, product);

        write_line("Product updated.");
    }
    else
    {
        write_line("Invalid index.");
    }
}

void sell_product(store_data &store)
{
    int index = read_integer("Enter the index of the product to sell: ");

    if (index >= 0 && index < length(store.products))
    {
        product_data product = get(store.products, index);

        if (product.stock > 0)
        {
            product.stock--;

            store.total_sales++;

            store.total_profits +=
                (product.sale_price - product.cost_price);

            set(store.products, index, product);

            write_line("Product sold.");
        }
        else
        {
            write_line("Out of stock.");
        }
    }
    else
    {
        write_line("Invalid index.");
    }
}

void print_menu()
{
    write_line("");
    write_line("===== STORE MENU =====");
    write_line("1. Add Product");
    write_line("2. Update Product");
    write_line("3. Delete Product");
    write_line("4. Sell Product");
    write_line("5. Print Store");
    write_line("6. Quit");
}

int main()
{
    store_data store;

    // Initialize totals
    store.total_sales = 0;
    store.total_profits = 0;

    int choice;

    do
    {
        print_menu();

        choice = read_integer("Choose an option: ");

        switch (choice)
        {
        case 1:
            add_product(store);
            break;

        case 2:
            update_product(store);
            break;

        case 3:
            delete_product(store);
            break;

        case 4:
            sell_product(store);
            break;

        case 5:
            print_store(store);
            break;

        case 6:
            write_line("Goodbye!");
            break;

        default:
            write_line("Invalid option.");
        }

    } while (choice != 6);

    return 0;
}
