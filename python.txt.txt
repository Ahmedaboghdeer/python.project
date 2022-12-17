import bs4
import requests
import csv
from itertools import zip_longest

url = 'https://wuzzuf.net/search/jobs/?q=digital+marketing&a=navbg'
page = requests.get(url)
result = page.content
# print(result)
soup = bs4.BeautifulSoup(result, 'html.parser')
# print(soup)
jobTitle = soup.findAll("h2", {"class": "css-m604qf"})
companyName = soup.findAll("a", {"class": "css-17s97q8"})
location = soup.findAll("span", {"class": "css-5wys0k"})
jobSkills = soup.findAll("div", {"class": "css-y4udm8"})
postNew = soup.findAll("div", {"class": "css-4c4ojb"})
postOld = soup.findAll("div", {"class": "css-do6t5g"})
posted = [*postNew, *postOld]
tittleList = []
companyList = []
skillsList = []
locationList = []
links = []
date = []
# salary = []
# requirmentsList = []
for t in range(len(jobTitle)):
    tittleList.append(jobTitle[t].text)
    links.append("https://wuzzuf.net" + jobTitle[t].find("a").attrs['href'])
    companyList.append(companyName[t].text)
    locationList.append(location[t].text)
    skillsList.append(jobSkills[t].text)
    date.append(posted[t].text)
for link in links:
    results = requests.get(link)
    srcs = results.content
    soups = bs4.BeautifulSoup(srcs, "html.parser")
    # requirments = soups.find("div", {"class": "css-1t5f0fr"})
    # salaries = soups.select("section div span span")
    # print(requirments)
    # salary.append(salaries)
print(date)
filelist = [tittleList, companyList, locationList, skillsList, links, date]
exported = zip_longest(*filelist)
with open("F:\iti\project.csv", "w") as myfile:
    writer = csv.writer(myfile)
    writer.writerow(["job tittle", "company name", "location", "skills", "links", "date"])
    writer.writerows(exported)
