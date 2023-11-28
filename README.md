# Web-scrapping-
import requests
from bs4 import BeautifulSoup
import csv

def scrape_product_data(url):
    # Send a GET request to the URL
    response = requests.get(url)

    # Check if the request was successful (status code 200)
    if response.status_code == 200:
        # Parse the HTML content of the page
        soup = BeautifulSoup(response.text, 'html.parser')

        # Extract product information (replace these with actual HTML tags from the target website)
        product_names = soup.find_all('div', class_='product-name')
        product_prices = soup.find_all('span', class_='product-price')
        product_ratings = soup.find_all('div', class_='product-rating')

        # Store the data in a list of dictionaries
        products_data = []
        for name, price, rating in zip(product_names, product_prices, product_ratings):
            product_data = {
                'Name': name.text.strip(),
                'Price': price.text.strip(),
                'Rating': rating.text.strip()
            }
            products_data.append(product_data)

        # Write the data to a CSV file
        with open('product_data.csv', 'w', newline='', encoding='utf-8') as csvfile:
            fieldnames = ['Name', 'Price', 'Rating']
            writer = csv.DictWriter(csvfile, fieldnames=fieldnames)

            # Write header
            writer.writeheader()

            # Write data
            writer.writerows(products_data)

        print("Data has been successfully scraped and saved to product_data.csv.")
    else:
        print(f"Failed to retrieve the page. Status code: {response.status_code}")

# Example URL (replace with the URL of the website you want to scrape)
example_url = 'https://example.com/products'
scrape_product_data(example_url)
