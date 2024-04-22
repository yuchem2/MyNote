## Basis
```R
setRepositories(ind = 1:7)

library(RSelenium)
library(rvest)
library(httr)
library(jsonlite)

library(data.table)
library(dplyr)
library(tidyverse)
library(janitor)

library(gganimate)
library(nord)
library(gifski)

selenium = "D:/data-minig-study/selenium"
workDir = "D:/data-minig-study/homework"
setwd(selenium)

## java -Dwebdriver.gecko.driver=”geckodriver.exe” -jar selenium-server-standalone-4.0.0-alpha-1.jar -port 4445
remDr <- remoteDriver(remoteServerAddr = "localhost" , 
                      port = 4445L,
                      browserName = "chrome")
```
## Question 1
> As already emphasized in class, most recently created web pages adopt a dynamic web operation method. Please select three web pages you frequently use and practice retrieving data from them. Since the data can be in various formats such as reviews/rankings, the more you practice on various web pages, the more advantageous you will have on the midterm exam.
 
```R
#### web 1
## crawling
url_web1 = "https://www.acmicpc.net/problemset?sort=ranking_asc"
remDr$open()
remDr$setWindowSize(width = 1920, height = 1080)

remDr$navigate(url_web1)
data <- c()
pages <- remDr$findElement(using = "xpath", value = '/html/body/div[2]/div[2]/div[6]/div[2]/div/ul')
pages$getElementText()[[1]] %>%
  strsplit(split = "\n") %>% unlist() -> pages
data <- data.frame()

for (page in pages) {
  remDr$navigate(paste(url_web1, "&page=", page, sep=""))
  
  page_source <- remDr$getPageSource()[[1]]
  # Sys.setlocale("LC_ALL", "English")
  page_source %>%
    read_html() %>%
    html_table() -> table
  # Sys.setlocale("LC_ALL", "Korean")
  data <- bind_rows(data, table)
}
remDr$close()
## end crawling
data -> Q1_web1
view(Q1_web1)

#### web 2
## crawling
url_web2 = "https://velog.io/"
remDr$open()
remDr$setWindowSize(width = 1920, height = 1080)

remDr$navigate(url_web2)
data <- c()

temp <- remDr$findElements(using = "class", value = "PostCard_block__FTMsy")
while(length(temp) <= 500) {
  remDr$executeScript("window.scrollTo(0, document.body.scrollHeight);")
  Sys.sleep(2)
  temp <- remDr$findElements(using = "class", value = "PostCard_block__FTMsy")
  print(length(temp))
}


for (x in temp) {
  data <- c(data, x$getElementText()[[1]] %>% strsplit("\n"))
}
remDr$close()

## simple cleansing
data <- data[0:500]
for (i in which(sapply(data, function(row) length(row)) != length(data[[1]]))) {
  data[[i]] <- append(data[[i]], NA, after = 1)
}
data <- as.data.frame(do.call(rbind, data))
rownames(data) <- c(1:500)
colnames(data) <- c("title", "preview", "date", "author", "likes")
data %>%
  separate(date, c("date", "comments"), sep = "·") -> Q1_web2
view(Q1_web2)


#### web 3
## crawling
url_web3 = "https://lol.ps/statistics"
remDr$open()
remDr$setWindowSize(width = 1920, height = 1080)
remDr$navigate(url_web3)
button <- remDr$findElement(using = "xpath", value = '//*[@id="section_tierlist"]/div[2]/div[1]/div[6]')
button$clickElement()

temp <- remDr$findElement(using = "xpath", value ='//*[@id="section_tierlist"]/div[3]/div[2]') 
temp <- temp$findChildElements(using = "tag", value = "a")
data <- c()

for(i in temp) {
  data <- c(data, i$getElementText()[[1]] %>% strsplit("\n"))
}
remDr$close()

## simple cleansing
Q1_web3 <- as.data.frame(do.call(rbind, data))
colnames(Q1_web3) <- c("Rank", "Changing", "Champion", "Lane", "PS Score", "Honey Score", "Win Rate", "Pick Rate", "Ban Rate", "Count")
view(Q1_web3)
```

## Question 2
> The address below is web page for games ranking 1-100 in 2023. Let’s crawl **the top 100 games from 2010 to 2024**, and organize the data in $100\times 15$ format as shown below. [https://www.metacritic.com/browse/game/](https://www.metacritic.com/browse/game/)

![|400](Pasted%20image%2020240422225907.png)

```r
#### Q2
url_game_origin = "https://www.metacritic.com/browse/game/all/all/"
url_game_remain = "/metascore/?page=1"
years = 2010:2024
remDr$open()

remDr$setWindowSize(width = 1920, height = 1080)

game_list <- list()

for (year in years) {
  games <- c()
  titles <- c()
  button <- c()
  
  url_temp <- paste(url_game_origin, year, url_game_remain, sep="")
  remDr$navigate(url_temp)
  Sys.sleep(2)
  
  for (i in 1:5) {
    Sys.sleep(2)
    titles <- remDr$findElements(using = "class", value = 'c-finderProductCard_titleHeading')
    for (title in titles) {
      text <- title$getElementText()[[1]]
      games <- c(games, text)
    }
    
    button <- remDr$findElement(using = "xpath", value = '//*[@id="__layout"]/div/div[2]/div[1]/main/section/div[4]/span[3]/span/span')
    button$clickElement()
  }
  
  game_list[[as.character(year)]] <- games[c(1:100)]
}
remDr$close()

data <- data.frame(game_list)
colnames(data) <- as.character(years)
Q2 <- data %>% mutate_all(~ gsub("^\\d+\\.\\s*", "", .))
view(Q2)
```
## Question 3
> Based on the data collected in (Q2), draw an animated barplot for the top 20 games from 2010 to 2013.
```R
#### Q3 
Q3 <- Q2 %>%
  select("2010":"2013") %>%
  pivot_longer(cols = everything(), names_to = "Year", values_to = "Game") %>%
  group_by(Year) %>%
  mutate(Rank = row_number()) %>%
  arrange(Year, Rank) %>%
  filter(Rank <= 20)
head(Q3)
output <- Q3 %>%
  ggplot() +
  geom_col(aes(x = Rank, y = Rank, fill = Game)) + 
  geom_text(aes(Rank, y = -1, label = Game), hjust = 1, vjust = 0.5, size = 6, color = "black", nudge_y = 0.2) +
  geom_text(aes(x = 1, y = 20, label = as.factor(Year)), vjust = 0.2, alpha = 0.5, col = "gray", size = 20) + 
  coord_flip(clip = "off", expand = FALSE) + scale_x_reverse() + 
  theme_minimal() + theme(
    panel.grid = element_blank(), 
    legend.position = "none",
    axis.ticks.y = element_blank(),
    axis.title.y = element_blank(),
    axis.text.y = element_blank(),
    axis.title.x = element_text(size = 20),
    axis.text.x = element_text(size = 15),
    plot.margin = margin(2, 3, 2, 15, "cm")
  ) +
  transition_states(Year, state_length = 2, transition_length = 2)
setwd(workDir)
animate(output, width = 1080, height = 960, fps = 25, duration = 15, rewind = FALSE,
        renderer = gifski_renderer("Q3Animated.gif"))
```
## Question 4
> Please crawl the tables on the following Wikipedia page via R Programming. [**https://en.wikipedia.org/w/index.php?title=IU_(singer)&diff=prev&oldid=1046571715**](https://en.wikipedia.org/w/index.php?title=IU_(singer)&diff=prev&oldid=1046571715)

```R
#### Q4
page = read_html("https://en.wikipedia.org/w/index.php?title=IU_(singer)&diff=prev&oldid=1046571715")
page %>%
  html_node('.wikitable') %>% html_table -> Q4
view(Q4)
```
## Question 5
> Let's cleansing the imported data in the form below.

![|400](Pasted%20image%2020240422225839.png)
```R
#### Q5
Q4 %>% 
  select(Year:Shows) %>%
  mutate(Title = gsub("\n.mw-parser-output .*$", "", Title)) %>%
  mutate(Title = gsub("\nDates.*$", "", Title)) %>%
  separate(Year, c("Year", "blank"), sep='–') %>%
  select(-blank) -> Q5

view(Q5)
```
## Question 6
> Let's check out how many countries have hosted concerts in total.

```R
#### Q6
Q5 %>% pull(Region) %>% n_distinct() -> Q6
cat("The number of unique regions is", Q6)
```
## Question 7
> Please summarize the total number of concerts held in each country in the following format

![|100](Pasted%20image%2020240422225938.png)
```R
#### Q7
Q5 %>% group_by(Region) %>%
  summarise(Shows = sum(Shows)) -> Q7
print(Q7)
```
## Question 8
> Which country has the highest number of concerts and which country has the lowest number of concerts?

```R
#### Q8
Q7 %>% 
  filter(Shows == max(Shows)) %>%
  pull(Region) -> Q8_max
Q7 %>%
  filter(Shows == min(Shows)) %>%
  pull(Region) -> Q8_min
  
cat("The country that hosted the most concerts is '", 
    paste(Q8_max, collapse = ", "), 
    "',\nand the country that hosted the least concerts is '", 
    paste(Q8_min, collapse = ", "), "'.",sep = "")
```
## Question 9
> Please crawl the table on the following Wikipedia page via R programming, then cleanse it in the following format: **[https://en.wikipedia.org/wiki/List_of_national_capitals](https://en.wikipedia.org/wiki/List_of_national_capitals)**

![|200](Pasted%20image%2020240422230023.png)

```R
#### Q9
page = read_html("https://en.wikipedia.org/wiki/List_of_national_capitals")
page %>% 
  html_node('.wikitable') %>% html_table -> Q9_craw_data
Q9_craw_data %>%
  select("Country/Territory", "City/Town") %>%
  rename(Region = "Country/Territory", Capital = "City/Town") %>%
  mutate_all(~ gsub("\\s*\\([^\\(]*\\)", "", .)) -> Q9

left_join(Q7, Q9, by = "Region") %>%
  select(Region, Capital) %>%
  group_by(Region) %>%
  summarise(Capital = paste(Capital, collapse = " | ")) %>%
  mutate(Capital = if_else(Capital == "NA", NA, Capital)) -> Q9

print(Q9)
```
## Question 10
> Let’s Merge the clean data created in **(Q5)** and **(Q9)** and organize them in the following format.

![|400](Pasted%20image%2020240422230125.png)

```R
#### Q10
left_join(Q5, Q9) %>%
  select("Year", "Title", "Region", "Capital", "Shows") -> Q10
view(Q10)
```
