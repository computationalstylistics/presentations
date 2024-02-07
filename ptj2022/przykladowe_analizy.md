



# Appendyks: przykłady



1. Rozkład statystyczny słowa _et_ w _Bellum Gallicum_ Juliusza Cezara (uwaga, potrzebny jest tekst, zapisany do pliku `caes_b-gall_1.txt`):


``` R
#setwd("Desktop/bootcamp/bootcamp_data/")
txt = readLines("caes_b-gall_1.txt")
tokens = unlist(strsplit(txt, "\\W+"))
tokens = tokens[nchar(tokens) >0]
# how many times the word "et" occurred?
sum(tokens == "et")
# how high is the percentage?
sum(tokens == "et") / length(tokens) * 100

split_txt = split(tokens, ceiling(seq_along(tokens)/300))
freq_in_samples = sapply(split_txt, function(x) sum(x == "et"))

hist()
density()
boxplot()

```


Plik z oryginalnymi danymi nie jest potrzebny; obliczone frekwencje są następujące:


``` R
freq_in_samples = c(12, 8, 8, 13, 11, 4, 6, 5, 8, 7, 10, 13, 7, 5, 9, 5, 5, 4, 2, 4, 7, 5, 4, 6, 13, 6, 4, 3)
hist()
density()
boxplot()

```


2. Rozkład statystyczny normalny dwóch niezależnych zmiennych:



``` R
plot(rnorm(10000) ~ as.numeric(rnorm(10000)), col = rgb(0,0,0,0.1))
```



3. Porównanie dwóch rozkładów prawdopodobieństwa; na przykładzie frekwencji formy _orłu_ oraz _orłowi_ w NKJP:



``` R
freq_orlowi = 20
freq_orlu = 10
# to save time, we compute but a fraction
grid = seq(0, 0.001, (1/300e7) )[1:500]
plot(dbinom(freq_orlu, size = 300e6, prob = grid), axes = FALSE, ylab = "probability")
axis(2)
axis(1, at = c(100, 200, 300, 400, 500), label = c(10, 20, 30, 40, 50))
box()
#
points(dbinom(freq_orlowi, size = 300e6, prob = grid), col = "red")
#
```


4. Porównanie dwóch rozkładów; na podstawie frekwencji zaimka _się_ w 100 powieściach polskich oraz u Marii Dąbrowskiej:




``` R
###################################
# liczba wystąpień leksemu 'się' na 10000 słów


sie_w_100_powiesciach = c(267, 294, 339, 318, 294, 318, 213, 308, 337, 320, 333, 374, 219, 264, 248, 283, 291, 267, 278, 257, 254, 255, 236, 280, 278, 255, 298, 282, 264, 229, 279, 319, 291, 234, 290, 252, 287, 263, 294, 249, 273, 226, 299, 350, 347, 315, 277, 338, 252, 251, 207, 275, 230, 265, 250, 260, 275, 234, 266, 260, 285, 325, 308, 253, 231, 272, 216, 252, 230, 289, 255, 268, 337, 293, 289, 305, 288, 250, 255, 260, 258, 273, 244, 222, 255, 301, 266, 264, 297, 301, 312, 259, 190, 217, 277, 244, 230, 288, 310, 262)

dabrowska = c(318, 341, 308, 298, 323, 331, 315, 293, 331, 342, 301, 337, 306, 312, 336, 318, 312, 290, 326, 325, 313, 304, 313, 301, 318, 321, 326, 320, 309, 343, 327, 338, 335, 314, 320, 320, 282, 307, 342, 315, 353, 329, 355, 344, 339, 337, 326, 337, 352, 301, 315, 365, 332, 360, 319, 359, 337, 322, 357, 365)


plot(density(sie_w_100_powiesciach))

boxplot(sie_w_100_powiesciach, dabrowska)


istotnosc = mean(sie_w_100_powiesciach) + (1.96 * sd(sie_w_100_powiesciach))
t.test(sie_w_100_powiesciach, dabrowska)


```



5. Dotatkowe materiały, m.in. prawo Zipfa na danych gramatycznych:


https://computationalstylistics.github.io/zipf_on_grammar/


6. Prawo Zipfa, prawo Heapsa:



``` R
library(stylo)
data(novels)

txt = unlist(strsplit(tolower(novels[[1]]), "[^a-z]+"))

# zipf
freq = sort(table(txt), decreasing = TRUE)
plot(as.numeric(freq), log = "xy")



# zipf II
plot(nchar(names(freq)), log = "x")



# heaps
results = NULL
for(i in 2:length(txt)) { 
    seen = txt[i] %in% txt[1:(i-1)]
    results = c(results, seen)
}
plot(cumsum(!results))
```

