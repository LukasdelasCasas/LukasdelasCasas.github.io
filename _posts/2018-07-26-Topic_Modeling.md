---
layout: post
title: Topic Modeling with R
image: /img/lukas_voit/Profil_Voit.JPG
---

At first i ran the hole script on a base of 1000 articles to see if i got the dendrogram and the topic graphs as a result. 

After a small change in the script -provided by Dr. Romanov-, i finally also got the graphs (line 185 had to be copied to work on windows).

In a second step i included a for loop to itinerate through the different number of topics. The script with the included for- loop solution i created can be seen here:

```R
topic.var <- c(1:30)
for (i in topic.var) {
  topic.var = i
  plotting.data = topics.texts.stats.cumulative
  plotting.data.var = count(plotting.data, vars=c("date"), wt_var = sprintf("V%d", topic.var))
  names(plotting.data.var) = c("date", "var")
  plotting.data.tot = count(plotting.data, vars=c("date"), wt_var = "total")
  names(plotting.data.tot) = c("date", "tot")
  plotting.data.fin = merge(plotting.data.var, plotting.data.tot, by="date")

  ggplot(plotting.data.fin, aes(x=date, y=var/tot*100)) +
     geom_line(aes(y=var/tot*100), col="gray") +
     geom_smooth(method="loess", span = 0.1, col="blue") +
     labs(title=paste0("Topic ",topic.var," Over Time"),
       subtitle=paste("Topic Words:", topics.labels[topic.var,][2]),
       caption="Source: Richmond Dispatch (Americal Civil War Period)",
       y="Frequencies %")

# save the graph
  graph.name = paste0(file.prefix, "Topic_", topic.var, ".png")
  ggsave(graph.name, width = 10, height = 5, dpi = 300)

# Now print out a few top samples of a topic
  topics.texts.stats.complete = topics.texts.stats.complete[order(-topics.texts.stats.complete[,topic.var+1]),]
  topics.texts.stats.complete$text[1:5]
}
```


After i checked that my loop was working, i altered the number of reinterations from 100 to 500. This step was made to optimize the results, but had the side effect that it took a lot more time to run the script. 

# Matching my results with each other and with Nelsons Dispatch:

Now i also changed the number of topics from 40 to 30 and compaired the results with each other. I could count severall observations:

There a quite some similarities between those different numbers of topics. I could find several topics, that are almost or close to being identical.
In both cases the graphs were contructed out of a group of 15 predictive words for each topic. 

The graphs and keywords of Topic Nr. 40/1 and Nr. 30/30 are almost identical, although the scale of frequencies is slightly different:

Topic 40 Nr.1 (Death Notices):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_1.png)

Topic 30 Nr. 30 (Death Notices):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_30.png)

This results can be matched with those of Rob Nelson’s Mining the Dispatch through use of the predictive keywords. In this case my results can be compaired with the topic _"death notices"_.
The graph of Nelson is partly identical with my graphs. The slight differences can be explained through the notion that Nelson is using a higher number of predictive words -24 to be exactly. 

I came to the same conclusion for several other topics. For example the Topic Trade of Nelsons Dispatch is matching both the graphs from my two runs. 
Furthermore several dissimilarities between the graphs can be explained with the nonidentical scaling of my graphs and the different layout as in case of Nelsons graphs.

Topic 40 Nr.2 (Trade):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_2.png)

Topic 30 Nr. 20 (Trade):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_20.png)

Anouther example: 

Topic 40 Nr.10 (Fugitive Slave Ads):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_10.png)

Topic 30 Nr. 21 (Fugitive Slave Ads):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_21.png)

In this cases the my graphs can be easily matched with Nelsons _"Trade"_ and _"Fugitive Slave Ads Topic"_. Another case is this:

Topic 40 Nr.2 (Patriotism):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_7.png)

Topic 30 Nr. 20 (Patriotism):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_9.png)

Same can be said in this cases:
Topic 40 Nr.25 (Maritime News):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_25.png)

Topic 30 Nr. 26 (Maritime News):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_26.png)

Topic 40 Nr.30 (Court proceedings):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_30.png)

Topic 30 Nr. 5 (Court proceedings):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_5.png)


Topic 40 Nr.22 (Military Recruitment):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_22.png)

Topic 30 Nr. 23 (Military Recruitment):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_23.png)


The frequency with 30 Topics is slightly higher in this case. Compaired to Nelsons _"Poetry and Patriotism"_ Topic the visualized graphs are close to being identical,
but this is a wrong impression as the frequencies of both my graphs are higher. This can be explained with the already mentioned small number of keywords. Even more Nelson
has given the user the option to exclude articles and adverticements by highering the threshold, because many of those are only matched by two or three words. This can _"add up"_
as Nelson explains it and this has at least to be noted or else information can be misinterpretated. 

Three more examples with this kind of observation can be seen here:

1.
Topic 40 Nr.2 (War Reports):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_11.png)

Topic 30 Nr. 20 (War Reports):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_13.png)

2. 
Topic 40 Nr.17 (War Bonds):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_17.png)

Topic 30 Nr. 25 (War Bonds):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_25.png)

3. 
Topic 40 Nr.40 (Clothing Ads):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_40.png)

Topic 30 Nr. 29 (Clothing Ads):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_29.png)



Of course there are also visualisations created by the R-script which don´t match as perfect as the examples shown above. 
In this examples Nelson Dispatch can´t be really compaired and mostly only partly match my results.

This can be said for the following manualy matched topics:

Topic 40 Nr.2 (Trade):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_4.png)

Topic 30 Nr. 20 (Trade):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_20.png)

This Topics:

Topic 40 Nr.14 (Confederate States):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_14.png)

Topic 30 Nr. 8 (Confederate States):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_8.png)


This Topic:
Topic 40 Nr.34 (Government):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_34.png)

Topic 30 Nr. 6 (Government):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_6.png)

This Topic:
Topic 40 Nr.16 (Legislature):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_16.png)

Topic 30 Nr. 18 (Legislature):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_18.png)

No resembles with Nelsons Dispatch:
Topic 40 Nr.38:
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_38.png)

Topic 30 Nr. 3:
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_3.png)



This Topic doesn´t match any of Nelsons results and my own visualisations differ from each other:
Topic 40 Nr.35 :
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_35.png)

Topic 30 Nr. 19 (Legislature):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_19.png)


Those two don´t  really match each other, but the Nr. 40/ 4 shows similarities with the dispatch of Nelson:
Topic 40 Nr.4 (Legislature):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_4.png)

Topic 30 Nr. 12 (Legislature):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_12.png)

Same can be accounted for Nr. 40/19:
Topic 40 Nr.19 (European News):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_19.png)

Topic 30 Nr. 1 (European News):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_1.png)

Same issue is accountable for Nr. 40/23, which is more comparable with Nelson:

Topic 40 Nr.23 (Groceries):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_23.png)

Topic 30 Nr. 24 (Groceries):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_24.png)


This can be explained with the -already mentioned- low number of predictive keywords in my run. In those cases small changes can have a great impact on the results. 
The same can be said about high number of keywords in topic modeling. This will create misleading results.

There are also cases which match each other but not the results Nelson got as seen here:

Topic 40 Nr.26:
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_26.png)

Topic 30 Nr. 22:
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_22.png)


But as writen above this mismatch with Nelson can also be an issue with the scaling.

At last i have also to say, that some differences between my results and Nelson Dispatch can be explained with the arrangement of the graph.
While my graph is constructed with the average of the results (blue line) and minima and maxima are depicted separatly in grey, Nelsons has also the minima and maxima in his visualisation incluided.
This also can explain in some cases the nonidentical visualisations. For example:

Topic 40 Nr. 33 (For hire and wanted ads):
![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_33.png)

Topic 30 Nr. 14 (For hire and wanted ads):
![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_14.png)


I couldn´t match the following results as the predictive keywords differ to far from another:

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_12.png)

![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_28.png)

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_3.png)

![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_2.png)

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_5.png)

![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_4.png)

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_6.png)

![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_7.png)

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_13.png)

![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_10.png)

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_9.png)

![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_11.png)

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_18.png)

![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_15.png)

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_15.png)

![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_17.png)

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_18.png)

![](../img/lukas_voit/tm30_A20_50_iter500_Max10_Topic_27.png)

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_20.png)

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_21.png)

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_24.png)

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_27.png)

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_28.png)

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_29.png)

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_31.png)

![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_32.png)


![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_36.png)


![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_37.png)


![](../img/lukas_voit/tm40_A20_50_iter500_Max10_Topic_39.png)


Moreover i cound´t recreate some topics, which i belive is caused by the low number of keywords. This can be observed on some of Nelsons Topics.
For example the topics Weather, Humor and Religion aren´t matched by my visualisations. 

As a final observation i can note that in general -iin my opinion- there is not a big difference between 30 and 40 topics.
Clearly it has an influence as over 10 topics aren´t created and further a higher number of topics matches Nelsons topics even more.
This can be explained with the small number of keywords used in my run. On the contrary Nelson altered the number by nine to 24, which garanties a better result as long as the number is not to high.
Same can be said about a low number of keywords which only partially can creat a valid result as it can not fully match every article. Furthermore it can be difficult to distinguish between topics. 
