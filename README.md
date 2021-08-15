# Python Automated Bulk WhatsApp Messages

It is a python script that sends WhatsApp message automatically from WhatsApp web application with saved contact numbers. It can be configured to send advertising messages to customers. It read data from an excel sheet and send a configured message to people.

## Demo
* Video clip on youtube of the script execution. https://youtu.be/NcWXpsczl3c

## Note
This is for saved contact numbers only if you want to send whatsapp bulk messages to unsaved or without saving the contact numbers. You may prefer another repository.
* Repository: https://github.com/inforkgodara/whatsapp-bulk-messages-without-saving-contacts
* Demo on Youtube: https://youtu.be/KBe26Kk5d9c

## Important
* If this repository helped you to understand at least something new please give star this repository which motivates me to work further for the similar kinds for projects.

## Prerequisites

In order to run the python script, your system must have the following programs/packages installed and the contact number should be saved in your phone (You can use bulk contact number saving procedure of email). There is a way without saving the contact number but has the limitation to send the attachment.
* Python 3.8: Download it from https://www.python.org/downloads
* Selenium Web Driver: Either you can use repo driver else you can download it https://chromedriver.chromium.org/downloads
* Google Chrome : Download it from https://www.google.com/chrome
* Pandas : Run in command prompt **pip install pandas**
* Xlrd : Run in command prompt **pip install xlrd**
* Selenium: Run in command prompt **pip install selenium** 

## Approach
* User scans web QR code to log in into the WhatsApp web application.
* The script reads a customized message from excel sheet.
* The script reads rows one by one and searches that contact number in the web search box if the contact number found on WhatsApp then it will send a configured message otherwise It reads next row. 
* Loop execute until and unless all rows complete.

Note: If you wish to send an image instead of text you can write attachment selection python code.

## Legal
* This code is in no way affiliated with, authorized, maintained, sponsored or endorsed by WhatsApp or any of its affiliates or subsidiaries. This is an independent and unofficial software. Use at your own risk. Commercial use of this code/repo is strictly prohibited.

## Code
```
# Program to send bulk customized message through WhatsApp web application
# Author @inforkgodara

from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.action_chains import ActionChains
from selenium.common.exceptions import NoSuchElementException
import pandas
import time

# Load the chrome driver
driver = webdriver.Chrome()
count = 0

# Open WhatsApp URL in chrome browser
driver.get("https://web.whatsapp.com/")
wait = WebDriverWait(driver, 20)

# Read data from excel
excel_data = pandas.read_excel('Customer bulk email data.xlsx', sheet_name='Customers')
message = excel_data['Message'][0]

# Iterate excel rows till to finish
for column in excel_data['Name'].tolist():
    # Locate search box through x_path
    search_box = '//*[@id="side"]/div[1]/div/label/div/div[2]'
    person_title = wait.until(lambda driver:driver.find_element_by_xpath(search_box))

    # Clear search box if any contact number is written in it
    person_title.clear()

    # Send contact number in search box
    person_title.send_keys(str(excel_data['Contact'][count]))
    count = count + 1

    # Wait for 3 seconds to search contact number
    time.sleep(3)

    try:
        # Load error message in case unavailability of contact number
        element = driver.find_element_by_xpath('//*[@id="pane-side"]/div[1]/div/span')
    except NoSuchElementException:
        # Format the message from excel sheet
        message = message.replace('{customer_name}', column)
        person_title.send_keys(Keys.ENTER)
        actions = ActionChains(driver)
        actions.send_keys(message)
        actions.send_keys(Keys.ENTER)
        actions.perform()

# Close Chrome browser
driver.quit()
```
Note: The script may not work in case if the HTML of web WhatsApp is changed.

Find it on youtube. https://youtu.be/NcWXpsczl3c
