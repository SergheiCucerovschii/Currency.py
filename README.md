# import xml.etree.ElementTree as ET
import requests
import bs4
import datetime


class Currency:
    def __init__(self, id, code, nominal, rate):
        self.id = id
        self.code = code
        self.nominal = nominal
        self.rate = rate

    def __str__(self):
        return f"\n " \
               f"Id: {self.id}\n " \
               f"Code: {self.code}\n " \
               f"Nominal: {self.nominal}\n " \
               f"Rate: {self.rate}\n "

    def __repr__(self):
        return str(self)


class CurrencyService:
    time_now = datetime.datetime.now()

    url = ('https://www.bnm.md/ru/official_exchange_rates?get_xml=1&date=19.07.2021')
    url_code = requests.get(url)
    soup = bs4.BeautifulSoup(url_code.text, 'lxml')

    valuteId = soup.find_all('numcode')
    code = soup.find_all('charcode')
    nominal = soup.find_all('nominal')
    rate = soup.find_all('value')

    currency_list = []
    for i in range(0, len(valuteId)):
        rows = [valuteId[i].get_text(),
                code[i].get_text(),
                nominal[i].get_text(),
                rate[i].get_text()]
        currency_list.append(rows)
    print(currency_list[:4])


    # def get_data(xml_url):
    #     try:
    #         web_file = urllib.request.urlopen(xml_url)
    #         return web_file.read()
    #     except:
    #         pass
    #
    # def get_currencies_dictionary(xml_content):
    #
    #     dom = minidom.parseString(xml_content)
    #     dom.normalize()
    #
    #     elements = dom.getElementsByTagName("Valute")
    #     currency_list = []
    #     print(len(elements))
    #     for i in elements:
    #         currency_dict.append(Currency(elements[i].find('NumCode').text, elements[i].find('CharCode').text,
    #             elements[i].find('Nominal').text, elements[i].find('Value').text))
    #     return currency_list
    #     # for node in elements:
    #     #     for child in node.childNodes:
    #     #         if child.nodeType == 1:
    #     #             if child.tagName == 'Value':
    #     #                 if child.firstChild.nodeType == 3:
    #     #                     value = float(child.firstChild.data.replace(',', '.'))
    #     #             if child.tagName == 'CharCode':
    #     #                 if child.firstChild.nodeType == 3:
    #     #                     char_code = child.firstChild.data
    #     #     currency_dict[char_code] = value
    #     # return currency_dict
    #
    # # def print_list(dict):
    # #     for key in dict.keys():
    # #         print(key, dict[key])
    #
    # url = 'https://www.bnm.md/ru/official_exchange_rates?get_xml=1&date=19.07.2021'
    # currency_dict = get_currencies_dictionary(get_data(url))
    # # print_dict(currency_dict)
