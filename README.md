# MIME Diversity in the Text Retrieval Conference (TREC) Polar Dynamic Domain Dataset
This is the website of my findings of the MIME Diversity in the Text Retrieval Conference (TREC) Polar Dynamic Domain Dataset. This was for the assignment of [CS 599 Content Detection and Analysis for Big Data] (http://sunset.usc.edu/classes/cs599_2016/) taught by [Dr. Chris Mattmann](http://sunset.usc.edu/~mattmann/). More details about the assignment can be found [here](http://sunset.usc.edu/classes/cs599_2016/CS599_HW_MIME_POLAR.pdf).

## About
I've worked with majorly 15 different mime-types from the TREC Polar Dataset. The data was parsed by using [Apache Tika](https://tika.apache.org/). The supported mime-types are as follows:
+ application/mp4
+ application/msword
+ application/octet stream
+ application/pdf
+ application/rss+xml
+ application/xhtml+xml
+ application/xml
+ audio/mpeg
+ image/gif
+ image/jpeg
+ image/png
+ text/html
+ text/plain
+ video/mp4
+ video/quicktime
I worked with around 50k-100k files for each mime type to generate the fingerprints.

4 major byte analysis were done on the mime types.

1. **Byte Frequency Analysis (BFA)**: Much like text documents can be broken into series of letters, all computer files can be broken into a series of bytes, which correspond to eight-bit numbers capable of representing numeric values from 0 to 255 inclusive. By counting the number of occurrences of each byte value in a file, a frequency distribution can be obtained. Many file types have consistent patterns to their frequency distributions, providing information useful for identifying the type of unknown files.

2. **Byte Frequency Distribution Correlation**: The frequencies of some byte values are very consistent between files of some file types, while other byte values vary widely in frequency. This suggests that a “correlation strength” between the same byte values in different files can be measured, and used as part of the fingerprint for the byte frequency analysis. In other words, if a byte value always occurs with a regular frequency for a given file type, then this is an important feature of the file type, and is useful in identification. A correlation factor can be calculated by comparing each file to the frequency scores in the fingerprint. The correlation factors can then be combined into an overall correlation strength score for each byte value of the frequency distribution.

3. **Byte Frequency Cross Correlation**: While the above 2 approaches compares overall byte frequency distributions, other characteristics of the frequency distributions are not addressed. Cross-correlation between byte value frequencies can be measured and scored to strengthen the identification process.

4. **File Header Trailer (FHT) Analysis**: The above approaches make use of byte value frequencies to characterize and identify file types. While these characteristics can effectively identify many file types, some do not have easily identifiable patterns. To address this, the file headers and file trailers can be analyzed and used to strengthen the recognition of many file types. The file headers and trailers are patterns of bytes that appear in a fixed location at the beginning and end of a file respectively. These can be used to dramatically increase the recognition ability on file types that do not have strong byte frequency characteristics.

## Learnings
1. **Byte Frequency Analysis**: From BFA we can know which are the bytes that are important in the mime type and which occur the most. For example: If we consider the BFA of the `text/plain` then we notice that the byte 32 (Space) occurs quite a lot of times. So this can give us an indication, that if we have more spaces in a particular BFA fingerprint of a file, then that could be an indication that its a `text/plain` file.

2. **Byte Frequency Distribution Correlation**: Consider the analysis of `text/plain`. We note that almost all of the data in the files shown in Figure 2-1 lie between byte values 32 and 126, corresponding to printable characters in the lower ASCII range. This is characteristic of the plain text files format. On the other hand, the data within the byte value range corresponding to the ASCII English alphanumeric characters varies widely from file to file, depending upon the contents of the file. This gives us the indication of the byte values that occur with regular frequency for a given file type.

3. **Byte Frequency Cross Correlation**: Consider the BFA for `text/html`. There are two equal-sized spikes in the frequency scores at byte values 60 and 62, which correspond to the ASCII characters “<” and “>” respectively. Since these two characters are used as a matched set to denote HTML tags within the files, they normally occur with nearly identical frequencies. Byte value pairs that have very consistent frequency relationships across files, such as byte values 60 and 62 in HTML files as mentioned above, will have a high correlation strength score. Byte value pairs that have little or no relationship will have a low correlation strength score.

   If we consider the cross-correlation matrix for `text/html` files, note that there are ranges of byte values in the frequency distribution that never occurred in any files (they appear with 0 frequency.) These regions appear in the cross-correlation plot as white regions of 0 frequency difference, and dark red regions with a correlation strength of 1. Furthermore, the intersection of 60 on the vertical axis with 62 on the horizontal axis (corresponding to the ASCII values for the “<” and “>” characters as mentioned above) shows a dark red dot representing a correlation strength of 1, as expected.

   If we consider the matrix for `image/gif` files then we see that the sawtooth pattern shown in the frequency distributions are a characteristic feature of the GIF file type, and it manifests in the cross-correlation plot as a subtle grid pattern in the frequency difference region. It is also clear that the regions of high and low correlation strength are much more evenly distributed in the GIF file type than the HTML file type. By seeing the pattern for gif files, we can classify the file as a GIF file if we see a similar pattern for an unknown mime type.

4. **File Header Trailer (FHT) Analysis**: The first few bytes of the GIF header show high correlation strengths (represented by dark red marks,) indicating that this type has a strongly characteristic file header. The specification for the GIF format states that the files shall all begin with the text string “GIF87a” for an earlier version of the format, or “GIF89a” for a later version.

   Further inspection shows that byte position 0 has a correlation strength of 1 for byte value 71, which corresponds to the ASCII character “G”. The second byte position has a correlation strength of 1 for byte value 72, which corresponds to the ASCII character “I”. This continues through byte values 70 (ASCII “F”) and 56 (ASCII “8”). At byte position five, though, byte values 55 and 57 both show correlation strengths roughly balanced. This indicates that these byte values, which correspond to the ASCII values for “7” and “9” respectively, occurred with almost equal frequency in the input files. (This further indicates that approximately equal numbers of files of each version of the GIF format were loaded into the fingerprint.) Finally, byte position six shows a correlation strength of 1 for byte value 97, corresponding to the ASCII character “a”. 
   
   Beyond byte position six, there is a much broader distribution of byte values, resulting in lower correlation strengths and lighter marks on the plot. So we cannot take that as a magic byte pattern. Similarly, for `image/jpeg` files the string `JFIF` appears at an offset of 6. These are strong indicators of the file mime-type.
   
## Screenshots
1. BFA `text/html`
   ![BFA text/html](screenshots/bfa-text-html.png)
2. BFA `text/plain`
   ![BFA text/plain](screenshots/bfa-text-plain.png)
3. Cross Correlation `image/gif`
   ![Cross Correlation image/gif](screenshots/cross-correlation-image-gif.png)
4. Cross Correlation `text/html`
   ![Cross Correlation text/html](screenshots/cross-correlation-text-html.png)
5. FHT `image/gif`
   ![Cross Correlation image/gif](screenshots/fht-image-gif.png)
