# Video Srt Operation

This Django application allows users to upload a video to an S3 bucket, parse the subtitle using ccextractor, and upload the parsed subtitle into DynamoDB. The application also provides a search interface that allows users to search for subtitles and receive a corresponding video with the matching subtitle and time frame. The development of this application involved optimizing the database and network, reducing latency, and designing an efficient architecture. 

**Note that the repository has been archived until it can be reviewed by the designated party, but the working sample and approach are still available for review.**


## Database design
* Filename (Partition Key)
* Time Period (Sort Key)
* CC (subtitle)


## Optimization
 
### Architectural approach (Why implemented db design improves performance)
**Why make filename partition key and time as sort key?**

**Answer:**  *In DynamoDB each block contains rows with the same partition key.
Searching for rows with a particular partition key value can be faster because it avoids scanning the entire table.
Using shorter partition and sort keys can also help to reduce the space required to store the keys and minimize data transfer when retrieving items from the table.*

**Why not make cc column partition key since it would allow one to use query instead of scan?**

**Answer:** 

```The maximum size of a partition key in DynamoDB is 2048 bytes```

*If a partition key exceeds this limit, it may not be possible to store it in the database. It is possible that a subtitle, which is a text string used to provide translations or descriptions of a video's content, could exceed this limit and therefore may not be suitable to use as a partition key in DynamoDB. It is important to consider the size of the partition key when designing a table in DynamoDB to ensure that it can be stored efficiently and effectively.*

### Coding approach ###
**What has been implemented on code level to optimize performance?**

**Answer:** *An offset endpoint has been implemented to limit the amount of data returned at a time, improving network performance and reducing strain on the database. This offset feature also helps to manage data during scan operations, improving database efficiency.*

## Working

#### General Working ####

(https://user-images.githubusercontent.com/66101153/211189703-b69124d8-aae8-4e0f-871a-d7bdf52dd1b0.mp4)


#### Validation Testing ####

*Forging*
[![Forging](https://user-images.githubusercontent.com/66101153/211189718-46a98645-8b20-45cb-9553-5651ecf72789.mp4)]

*Pre existent*
(https://user-images.githubusercontent.com/66101153/211189823-4357f87d-9fe9-42a6-b77c-efdba8668bee.mp4)


#### Latency Testing ####

(https://user-images.githubusercontent.com/66101153/211189794-e5251213-66ec-4c5b-a27d-646c198c01f5.mp4)


#### Celery Working ####

(https://user-images.githubusercontent.com/66101153/211189741-fadd242d-5896-489a-8c70-beea081f41a4.mp4)






