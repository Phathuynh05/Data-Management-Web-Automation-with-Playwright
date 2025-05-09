# Data Srapping & Web Automation with Playwright

    from playwright.sync_api import sync_playwright
    from tabulate import tabulate
    
    def scrape_nike_mens_shoes():
        with sync_playwright() as p:
            browser = p.chromium.launch(headless=False, slow_mo=150)  # slow down to look human
            context = browser.new_context(
                user_agent='Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 Chrome/114.0.0.0 Safari/537.36',
                viewport={'width': 1280, 'height': 800}
            )
            page = context.new_page()
            
            print("Navigating to Nike Men's Shoes page...")
            page.goto('https://www.nike.com/w/mens-shoes-nik1zy7ok', wait_until="domcontentloaded")
    
            print("Waiting for products...")
            page.wait_for_selector('div.product-card__info')
    
            products = page.query_selector_all('div.product-card__info')
    
            results = []
    
            for product in products:
                title_elem = product.query_selector('div.product-card__title')
                subtitle_elem = product.query_selector('div.product-card__subtitle')
                price_elem = product.query_selector('div.product-price')
    
                title = title_elem.inner_text().strip() if title_elem else '[No Title]'
                subtitle = subtitle_elem.inner_text().strip() if subtitle_elem else '[No Subtitle]'
                price = price_elem.inner_text().strip() if price_elem else '[No Price]'
    
                results.append([title, subtitle, price])
    
            if results:
                print("\n👟 Nike Men's Shoes:\n")
                print(tabulate(results, headers=["Product Name", "Category", "Price"], tablefmt="fancy_grid"))
            else:
                print("⚠️ No products found.")
    
            browser.close()
    if __name__ == "__main__":
        scrape_nike_mens_shoes()
    
    if __name__ == "__main__":
        scrape_iphone_storage_options()
