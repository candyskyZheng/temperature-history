import requests
from bs4 import BeautifulSoup
import bs4
import pygal
import re

# 输入
place = input('请输入要查询的地点（汉语拼音）：')
year_month = input("请输入要查询的时间（年份月份, 例'201902'）：")
url = 'http://www.tianqihoubao.com/lishi/{}/month/{}.html'\
.format(place, year_month)


def getHTMLText(url) :
	try:
		r = requests.get(url, timeout = 30)
		r.raise_for_status()
		r.encoding = r.apparent_encoding
		return r.text
	except:
		return ""

# 获得气温数据
def qiwen(qiwen_list, html) :
	soup = BeautifulSoup(html, "html.parser")
	for tr in soup.find('table').children:
		if isinstance(tr, bs4.element.Tag) :
			tds = tr('td')
			qiwen_list.append([tds[2].string])

contents = []
def print_qiwen_List(qiwen_list, num) :
	for i in range(num) :
		u = qiwen_list[i]
		contents.append(u[0])

# 查询地点的名称
place_name = []
def Chinese_place_name(html):
	soup = BeautifulSoup(html, "html.parser")
	for title in soup.find('head').children:
		if isinstance(title, bs4.element.Tag) :
			place_name.append(title)

def main() :
	qiwen_list = []
	html = getHTMLText(url)
	qiwen(qiwen_list, html)
	print_qiwen_List(qiwen_list, len(qiwen_list))
	Chinese_place_name(html)

main()


cont0 = []
for content in contents[1:] :
	str1 = str(content[42:45])
	pattern = re.compile(r'\D+\d+|\d+')
	res = re.findall(pattern, str1)
	for i in res:
		i = int(i)
		cont0.append(i)

cont1 = []
for content in contents[1:] :
	str2 = str(content[129:134])
	pattern1 = re.compile(r'\D+\d+|\d+')
	res1 = re.findall(pattern1, str2)
	for i in res1:
		i = int(i)
		cont1.append(i)
	
dates = []
for x in range(1, len(cont0)+1):
	dates.append(str(x))

# 查询地点的中文名
a = str(place_name[:1]).split()
for i in a[1:2]:
	place_name = i[:2]

# 折线图
line_chart = pygal.Line(x_label_rotation=0)
line_chart.add('日最高气温', cont0)
line_chart.add('日最低气温', cont1)
line_chart.title = '{}{}历史气温'.format(year_month, place_name)
line_chart.x_labels = dates
line_chart.render_to_file('{} {} 历史气温.svg'.format(year_month, place_name))
