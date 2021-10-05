from requests_html import HTMLSession


def extract_product_data(product):
    """ Find the data for each product """
    product_name = product.find('.sku-header', first=True).text
    model = product.find('.sku-value')[0].text
    SKU = product.find('.sku-value')[1].text
    price = product.find('.priceView-hero-price', first=True).text

    # clean price data
    price = price.split('Your')[0]
    price = price.replace('$', '')

    return product_name, model, SKU, price


def create_product_dict(product_name, model, SKU, price):
    """ Create the product dictionary """
    product_dict = {'product_name': product_name,
                    'model': model,
                    'SKU': SKU,
                    'price': price}

    return product_dict


def check_for_discount(product, product_dict):
    """ See if the product has a discount """
    # check for discount
    discount = product.find('.pricing-price__savings', first=True).text

    # clean discount data
    discount = discount.replace('Save $', '')

    # add discount data to the JSON object
    product_dict['discount'] = discount

    return product_dict


def run_scraper(products):
    """ run the scraper """
    product_dict_list = []
    for product in products:
        # extract product data
        product_name, model, SKU, price = extract_product_data(product)

        # create JSON object for the product
        product_dict = create_product_dict(product_name, model, SKU, price)

        try:  # exception handler to avoid crashing on products with no discount data
            # add discount data
            product_dict = check_for_discount(product, product_dict)
        except AttributeError:
            pass

        # store the JSON object in list
        product_dict_list.append(product_dict)

    product_dict_list[:3]  # show the first three results

# start the session
session = HTMLSession()

# get the page response
url = 'https://www.bestbuy.com/site/promo/tv-deals'
response = session.get(url)
response_html = response.html

# create a new variable containing just product data
container = response_html.find('ol')

# create a list of products
products = response.html.find('.list-item')

# run the scraper
run_scraper(products)
