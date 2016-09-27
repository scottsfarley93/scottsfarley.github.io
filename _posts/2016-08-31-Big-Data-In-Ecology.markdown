---
layout: post
title:  "Ecological Data: Is it 'Big Data'?"
date:   2016-08-31 8:22:52 -0500
categories: Research Paleo
---

We've all heard the term 'Big Data', though it's often thrown around as a techy buzzword, along with others, like 'The Cloud', without a clear meaning.  In the Williams Lab, we're working with datasets that are sometimes called 'Big Data' in talks by [@iceageecolgist](https://twitter.com/iceageecologist) and others, housed in databases like [Neotoma](http://neotomadb.org), [the Global Biodiversity Information Facility](http://gbif.org), and the [Paleobiology Database](http://paleobiodb.org).  Today, I ask, what characteristics of our data make it 'Big Data'?


### Problem and Scope
> Question: Can ecological biodiversity data fit under the rubric of Big Data? If so, what are the characteristics that make it Big?

First, let's put a limit on the scope of the problem.  Ecology generally has many different subfields, each with their own data and data types.  Some of these may be particularly large, as in the case of ecological modelers, and some may be smaller.  For the sake of argument today, I'll limit the discussion to ecological biodiversity data documenting *occurrences*.   Each occurrence comes with metadata describing what species (typically, but could also be to another taxonomic grouping) was encountered, where it was encountered, and when it was encountered. This type of data is pervasive in the field, and can be used in a host of analyses, including modeling, climate change assessment, and hypotheses testing. Recently, there have been large international campaigns to aggregate these records into large, structured databases that facilitate global biodiversity syntheses.  Three that are commonly encountered are Neotoma (Quaternary), GBIF (modern and instrumental period), and PBDB (deep time).  Since PBDB's ```R``` package was hard to use, I investigate the question today using data from Neotoma and GBIF.


### Definitions of Big Data
There are two often-encountered, decidedly non-technical, designations of Big Data.  The first comes from Wikipedia

> Big data is a term for data sets that are so large or complex that traditional data processing applications are inadequate.
>
> (Wikipedia)

This is commonly seen in the marketing materials surrounding big computation and the cloud, though it's really not a definition at all.  It doesn't say much about what it is, just that 'traditional' means are not capable of processing it, pointing towards distributed computing, cloud computing, and other recent technological advances as its facilitator.  We do get a couple things from this definition though. We know that we're looking at discrete data sets, partitioned, presumably, in a logical manner.  We're looking for data sets that 'traditional' data processing applications are not feasible.  By using these words, 'large' and 'traditional', in particular, we can see that 'Big Data' is in the eye of the beholder, so to speak, and it depends on your tradition of data processing whether a new dataset is Big or not.  Guterman (2009) suggests, "for some organizations, facing hundreds of gigabytes of data for the first time may trigger a need to reconsider data management options. For others, it may take tens or hundreds of terabytes before data size becomes a significant consideration."  From Guterman's perspective, the focus is really on the number of bytes a dataset has, but as we'll see in a minute, there can be other important factors that comprise a data set's Bigness.

The second defintion comes from Yang and Huang's 2013 book Spatial Cloud Computing:
>Big Data refers to the four V's: volume, velocity, veracity, and variety.
>
>(Yang and Huang, 2013)

A varient of this definition can be traced back to an early IBM report on the topic, and can be seen in a variety of cheesy infographics, [like this one](http://www.ibmbigdatahub.com/infographic/four-vs-big-data).  Yang and Huang go on to further describe the meaning of the four V's, noting that "volume refers to the size of the data; velocity indicates that big data are sensitive to time, variety means big data comprise various types of data with complicated relationships, and veracity indicates the trustworthiness of the data" (p 276).  Here we get a bit more structure than the wikipedia definition gives us, and with the two together, we have a pretty good rubric on which to look at biodiversity datasets.

## Evaluation

#### Wikipedia
I argue that the very existence of complex relational databases, like GBIF, Neotoma and PBDB, suggest that biodiversity data do fall under the category of Big Data, as the traditional means of analyzing these data are possible anymore.  Of course, 'complex' in the context of the wikipedia statement typically refers to the preponderance of unstructured data, like videos and photos, and 'large' usually means too big to fit into a computer's memory and/or storage drives.  From this perspective, our data is not complex, rather it's stored in really organized relational tables, and fairly small (the entire Neotoma SQL dump can be downloaded at only 43MB).  

But, if we keep in mind that big data can mean different things to different people, then from our perspective in ecology, our data is Big. Consider the complexity of the relationships between different data records, for example. Figure 1 shows the Neotoma relational table structure, and the complicated web of relationships between each entity.  The data is both spatial and temporal, requiring these attributes, which are known to be messy (see "Veracity"), along with sample data and metadata.  Now, consider keeping track of this for tens of thousands (Neotoma) or hundreds of millions (GBIF) or records, among thousands of independent researchers, and we see why non-traditional techniques like these databases have been developed. Further developments, like APIs and R packages, are even more recent developments to further simplify the tasks of accessing, filtering and working with the datasets. No, ecological biodiversity data does not meet the scale and extent of YouTube, Twitter, or Amazon, but it does require new, custom built tools to store, analyze, and use.

[![Neotoma_ER](/assets/bigData/Neotoma_ER.jpg)](http://www.neotomadb.org/uploads/NeotomaDMD.pdf)
*Figure 1: Neotoma's Relational Table Structure*

#### Volume
Of the four V's, the one that most comes to mind when considering what is, or is not, Big Data is volume: how much data is there?  As the quote from Guterman (2009) suggests, some experts consider this to be the only factor in determining what makes data Big. Our datasets are not on the scale of billions of hours of YouTube videos or hundreds of billions of Tweets, but the scale of biodiversity data has exploded in recent years, bringing it to a place where the volume alone is challenging to manage.

Since the late 1990s, biodiversity databases have quickly and decisively increased the amount of data available to ecologists. Consider Figures 2 and 3, tracking the growth in collections of Neotoma and GBIF through time.  In 1990, only 2 of the records now stored in Neotoma were in digitized collections.  Today, there are over 14,000 datasets.  Each dataset is comprised of spatial and temporal metadata, along with one or more samples with data and associated metadata. The growth rate averages out to about 1.4 datasets every single day for over 26 years.  Considering the time, effort, and money that goes into working up a sediment core (or any of the other data types in Neotoma) this is a really impressive growth rate. For an interesting perspective on ecological Big Data's reliance on blood, sweat, and tears, take a look at this [Blog Post](https://contemplativemammoth.com/2013/07/10/is-pollen-analysis-dead-paleoecology-in-the-era-of-big-data/) by former Williams Labber Jacquelyn Gill.  

![Neotoma_Growth](/assets/bigData/Neotoma_Growth.png)
*Figure 2: Cumulative number of datasets in Neotoma*

The scale of GBIF is on an entirely different level than Neotoma (perhaps because some of the data gathering challenges faced in getting paleo data don't apply as strongly to modern data collection). Today, GBIF houses digital records of well over 500 million observations, recorded specimens (both fossil and living), and occurrences noted in the scientific literature. GBIF's records are largely comprised of museum collections, which allow their digital collection to date back to before 1900. The facility itself was introduced in 1999 and officially launched in 2001.  Since 2001, the facility's holdings have grown nearly 300%, from about 180 million in 2001 to just shy of 614 million occurrence records today.  Managing 613+ million records and associated metadata, and comping with such a fast growth rate, is, without a doubt, a data management challenge worthy of Big Data classification.  Figure 3 shows the exponential growth in GBIF's holdings since AD 1500, and Figure 4 is an interactive map showing the changes in spatial distribution of their observed data since the late 1800's.

![GBIF_Growth](/assets/bigData/GBIF_Growth.png)
*Figure 4: Exponential growth of occurrence records in GBIF*

<link rel="stylesheet" href="https://unpkg.com/leaflet@0.7.7/dist/leaflet.css" />
<script src="https://unpkg.com/leaflet@0.7.7/dist/leaflet.js"></script>
<script   src="https://code.jquery.com/jquery-3.1.0.slim.min.js"   integrity="sha256-cRpWjoSOw5KcyIOaZNo4i6fZ9tKPhYYb6i5T9RSVJG8="   crossorigin="anonymous"></script>
<div id='map' style='height:500px;'>
</div>
1890<input type='range' min='1890' max='2016' step='1' style='width:50%; display:inline-block; vertical-align:middle'
id='gbif_range' />2016
<script src="/assets/gbif_map.js"></script>

*Figure 4: Interactive -- Spatial Distribution of GBIF Holdings Through Time*

#### Variety
The second characteristic of Big Data in the four V's framework is the Variety of the data, and its 'various types with complicated relationships' (Yang and Huang). Biodiversity data is highly diverse with many very complicated relationships and interrelationships.

Neotoma's holdings range from XRF measurements, to geochronologic data, to fossil vertebrates, to modern pollen surface samples.  In total, there are 23 dataset categories in the database, with more being added from time to time. Though it is structured similarly in the database tables, each of these data types comes from a different community of researchers, using different methods and instruments. Figure 5 shows the breakdown of dataset types in the database.

![Neotoma_Record_types](/assets/bigData/Neotoma_types.png)
*Figure 5: Dataset Type Breakdown of Neotoma's Holdings*

GBIF has 9 defined record type categories, including human observation, living specimen, literature review, and machine measurements.  As with the Neotoma dataset types, these are wildly different from each other.  A living specimen is clearly a totally different type of data to work with than something was derived from a literature review. Yet all of these types coexist together in these large biodiversity datasets. Figure 6 shows how GBIF's records are distributed amongst these nine types.

![GBIF](/assets/bigData/gbif_types.png)
*Figure 6: Dataset Type Breakdown of GBIF Holdings*

To further add to the variety and complexity of our data, it is both spatial and temporal in nature, causing complicated interrelationships between data entities. 87.6 % of GBIF's records are georeferenced to a real place in the world. 100% of Neotoma's datasets have spatial information. In these databases, the spatial information is compounded by other fields that describe the location of the observation.  For example, Neotoma has fields describing the site where the fossil was found -- it's altitude, environment, area.  PBDB has extensive metadata for depositional environment, giving additional context to fossil occurrences.  GBIF often notes somewhat colloquial location descriptions in addition to geographic coordinates.   And, of course, there are the relationships between the spatial coordinates themselves -- are these things in the same place? do they overlap?

Managing data with a spatial component is nearly always more challenging than managing data without it. Figure 7 shows how the spatial locations of the datasets contained in Neotoma have changed through time.  Note the expansion in Europe and eastern Asia, and the lack of datasets in Africa.

[![Neotoma_Maps](/assets/bigData/neotoma_spatial_dist.png)](/assets/bigData/neotoma_spatial_dist.png)
*Figure 7: Spatial distribution of additions in Neotoma since 1990*

A final point on variety is that each record, though now cleanly structured and easily accessed as a record in a database, represents the work of an individual researcher.  The controlled vocabularies and organization policies enforced by the databases have helped to efficiently aggregate the data, however, nearly every record was collected, worked up, and published by a unique individual.  Figure 8 shows the number of datasets attributed to each PI in Neotoma.  Yes the names are too small to read.  The point, though, is that while a couple researchers have a very large number of datasets credited to them (John T Andrews has the most with 335), most have many fewer.  The median number of datasets contributed is 2, and the 3rd quartile value is just 7.  Each researcher will use different equipment, in a different way, call things different names, and generally just do things slightly differently -- yielding a highly variable dataset.

[![GBIF](/assets/bigData/Neotoma_PIs.png)](/assets/bigData/Neotoma_PIs.png)
*Figure 8: Neotoma dataset submissions by principle investigator*

#### Veracity
Ecological data has high levels of uncertainty associated with it.  Some can be estimated, like temporal and spatial uncertainty.  Others are less amenable to being quantified, for example inter-researcher identification differences, measurement errors, and data lost in the transition from field to lab to database. See [this paper](http://www.sciencedirect.com/science/article/pii/S0277379116300142#appsec1) for a Paleon project that used expert elicitation to quantify the differences between the dates assigned to European settlement horizon, a process they argue varies between sites, and depends on the "temporal density of pollen samples, time-averaging of sediments, the rapidity of forest clearance and landscape transformation, the pollen representation of dominant trees, which can dampen or amplify the ragweed signal, and expert knowledge of the region and the late-Holocene history of the site." The raw data from the expert elicitation is included as supplementary information in their paper, and can be seen to vary pretty significantly between the four experts.

 Some information will be lost in the process of going from a field site through a lab workflow to being aggregated in the dataset.  Not all process details can be incorporated into database metadata fields, and probably more importantly, contextual details essential to proper interpretation of the data often gets lost on aggregation.

Coincidentally, when I start working on my PhD here at UW, I'll be working to tackle some of these uncertainty issues.

To illustrate the veracity (or lack thereof) of the biodiversity data, let's look at spatial coordinate uncertainty in GBIF and temporal uncertainty of chronological control points in Neotoma. The GBIF database, in addition to recording the geographic coordiantes of an occurrence, also includes a field for uncertainty in spatial location, though this field is optional.  I downloaded 10,000 records of the genus *Picea*, of which over half did not include this field (though all were georeferenced).  This means that even if I am able include and propagate uncertainty in my models (as in Bayesian Hierarchical Models), I would be unable to do so really effectively, because few researchers even report this field. Of the 4,519 records that did report ```coordinateUncertaintyInMeters```, the average uncertainty was 305m (if you exclude zero, which seems reasonable to do). The maximum uncertainty in this dataset was 1,970m.  From this brief, and admittedly flawed, assessment, we can see there are some pretty serious problems with using the coordinates without considering their uncertainty first.  If, for example, you're using 800m gridded climate model output to look at environmental covariates to species presence (which I do), a 300m uncertainty in species location could cause significant deviations due to gridcell mis-assignment, particularly in mountainous regions like the Western U.S.

On the temporal side of things, we can do a similar assessment, this time using the Neotoma data.  Neotoma samples are assigned an age using age controls (like radiocarbon dates or varve counts) or an age model, which interpolates between the age controls. The age model issue is a challenging one, and there's a lot of literature out there about it, as well as software to improve from simple linear models. Every age model is based on a set of age controls, which often have uncertainty associated with them.   Neotoma records an minimum and maximum age for each age control for each dataset.  Out of a sample of 32,341 age controls in the database, only 5,722 reported age uncertainty.  Some record types, like varves, can perhaps be assigned an uncertainty of zero, so we can safely ignore 2,830 more controls, leaving us with 2,892 that report values for minimum and maximum age. The summary statistics for these age controls suggest that the median age model tie point has a temporal uncertainty of 260.0 years. The 25% percentile is an uncertainty of 137.5 years and the 75% 751.2 years.  Using the mean of 260.0 years, I suggest that we can only identify down to &plusmn; 130 years of the actual date.  Considering sediment mixing, laboratory precision, and other processes at work, maybe this isn't that big of a deal, but it definitely is something to be aware of and contributes to biodiversity data's lack of absolute veracity.

#### Velocity
The final piece of the framework is the data's velocity -- how time sensitive is the data.  Data's velocity important because high velocity data must be analyzed as a stream.  Tweets, for example, must be analyzed for trends as they are posted. Knowing the trending topics of two weeks ago might be interesting to me, but the real draw of a Big Data platform like twitter is that I can participate in the trending topics of *right now*.  To do such an analysis, one must use sophisticated sampling techniques and algorithms to detect clusters and trends in real time, for example [this paper](http://jmlr.csail.mit.edu/proceedings/papers/v17/bifet11a/bifet11a.pdf), which comments on sampling strategies used for trend detection.  

This is the one area where I would suggest that ecological biodiversity data is not Big Data.  Biodiversity analyses, like species distribution models, at least the ones I am familiar with, usually take between a few minutes and a few days to complete and are not especially time sensitive.  The rate of increase in data volume in both Neotoma and GBIF is not fast enough to invalidate the results from previous analyses.  Neotoma gets approximately 1.4 new datasets each day (1990-2016 average).  GBIF gets about 59,000 new occurrences each day (2000-2015 average).  Sure, that's a lot of new datasets, but the likelihood you would actually use the new data in a given analysis is low, and the likelihood that its immediately inclusion into a new model would significantly change your conclusions is even lower.

The velocity of data coming into the databases, particularly into GBIF, is staggering, no doubt about it.  Nonetheless, I don't think it it warrants the use of specialized streaming algorithms for extracting information from the new data points.  I have not seen anyone attempt to do such a thing (though maybe this would be an interesting experiment?).  Moreover, there is little incentive to immediately analyze the data, because there is next to nothing to be gained from modeling biodiversity faster than you can report your results in publications.  

### So, is it?
Velocity notwithstanding, biodiversity occurrence data passes four of five facets of the Big Data, so I conclude that, **yes, it is big data.** It requires specialized databases and software to interact with it, it has large numbers of records, it is extremely diverse, and it has high levels of uncertainty with which to deal.

Looking forward, I suspect Big Data will continue to challenge those involved in synthetic research. Perhaps one of the most challenging aspects is the relatively short period of time in which these data became Big. Figures 9 and 10 show the annual increase in holdings for Neotoma (Fig. 9) and GBIF (Fig 10) through time (top) and the rate of change of annual increase (bottom). While Neotoma's rate of increase as remained relatively steady through time (clear from the near-linear trend in Figure 2), GBIF's rate shows a significant upward trend in the last several years.

[![Neotoma_Delta](/assets/bigData/Neotoma_growth_diff.png)](/assets/bigData/Neotoma_growth_diff.png)

*Figure 9: Neotoma holdings, Annual Change and Rate of Change of Annual Change*

[![GBIF_Delta](/assets/bigData/gif_growth_diff.png)](/assets/bigData/gif_growth_diff.png)

*Figure 10: GBIF holdings, Annual Change and Rate of Change of Annual Change*
