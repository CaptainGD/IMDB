# IMDB
Data mining
import urllib.request
import re
def getAllcontents(address):
    AllContent = list()
    with urllib.request.urlopen(address) as urlcontent:
        for lines in urlcontent:
            AllContent.append(lines.decode('utf-8'))

    return(AllContent)



# Next we need to go over each line to find the
WholeContent = getAllcontents("https://www.imdb.com/chart/top?ref_=ft_250")

MovieAddress = list()
MovieName = list()

for i in range(0,len(WholeContent)):
    if re.search('<td class="titleColumn">', WholeContent[i]):
        MovieAddress.append('https://www.imdb.com' + WholeContent[i+2].split('href=')[1].split('\n')[0].split('"')[1])
        MovieName.append(WholeContent[i+3].split('</a>')[0].split('>')[1])

print(MovieName)


# Next we write a helper function to get the index line
# The input is list of content which contain the lines, return the index which contain the pattern
def getIndex(pattern, ContentList):
    resultList = list()
    for i in range(0,len(ContentList)):
        if re.search(pattern, ContentList[i]):
            resultList.append(i)
    return(resultList)

# Next get the content with the index
def getContent(index, ContentList):
    if len(index) > 0:
        return(ContentList[index[0]])
    else:
        return(None)

# Next we extract the number from the lines
def ExtractNumber(content):
    if content == None:
        return(None)
    else:
        tempcontent = content.replace(',', '').split('</h4>')[1]
        print(tempcontent)
        print(re.search(r'\d+', tempcontent))
        return(re.search(r'\d+', tempcontent).group(0))

OpeningUS = list()
Budget = list()
GrossUS = list()
GrossWorld = list()

for address in MovieAddress:
    MovieContent = getAllcontents(address)
    BudgetNum = getContent(getIndex(r'Budget',MovieContent),MovieContent)
    OpeningUSNum = getContent(getIndex(r'Opening Weekend USA',MovieContent),MovieContent)
    GrossUSNum = getContent(getIndex(r'Gross USA',MovieContent),MovieContent)
    GrossWorldNum = getContent(getIndex(r'Cumulative Worldwide Gross',MovieContent),MovieContent)
    print(ExtractNumber(BudgetNum))
    Budget.append(ExtractNumber(BudgetNum))
    OpeningUS.append(ExtractNumber(OpeningUSNum))
    GrossUS.append(ExtractNumber(GrossUSNum))
    GrossWorld.append(ExtractNumber(GrossWorldNum))

print(Budget)

# for address in MovieAddress:
#     print(getAllcontents(address))


# print(type(MovieAddress[]))
# for lines in :
#     if re.search(r'<td class="titleColumn">', lines):
#         print(lines)

# print(extractInfo(r'w', 'wwwwwwww5.565 88787').groups())



# for line in getAllcontents("https://www.imdb.com"):
#     print(extractInfo('[1-9]', line))
