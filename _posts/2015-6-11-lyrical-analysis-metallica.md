---
layout: post
title:  "Lyrical Analysis - Evolution of Metallica"
date:   2015-06-11
categories: [data, analysis, music]
---

## Abstract

This is a project that I've been putting off for a while, but I started to dive
back into it today. The idea is to take all of an artist's songs throughout
their career and count the frequency of word use for each year in which
they released albums. This set of data would essentially give us a sort of
lyrical histogram that we can use for further analysis.

Some potential questions & concepts I'd be interested in answering:

* How does the artist emotional state appear to fluctuate through their
career?
* Can we make the "word count" less granular and categorize words as
broader emotions (happy, angry, etc.)?
* After some research, can we see a correlation between artists' major life
events and the tone of their work at those times?

As the title suggests, I was originally going to write this as a one-off
research project focused on Metallica, but I figured why not make it generic?
This project should work with any artist whose lyrics MusixMatch has.

## Step 1) Find reliable source for lyrics data.

I'm currently using [MusixMatch](https://developer.musixmatch.com/), a
searchable lyrics service, because it provides a simple, free API that delivers
everything I need. In order to squeeze the data we want from their API, we
need to use several of its methods:

1. `artist.search` - find an artist by name, gives `artist_id`
2. `artist.albums.get` - get all of our artist's albums, gives `album_id`s
3. `album.tracks.get` - gets all of the tracks on a given album, gives `track_id`s
4. `track.lyrics.get` - gets the actual lyrics data for a given track

I wrote python utility functions to make interacting with the MusixMatch API
less painful, which you can check out
[here](https://github.com/unscsprt/lyrics-analysis/blob/master/lyrics.py).

My goal was to make it as modular as possible, and easy to use. The functions
mentioned in Step 2 require only the desired artist name to produce their
output. It couldn't be easier!

## Step 2) Hammer the data into a useful form.

Now that I had all of the functions I'd needed, I got to work on using
them to retrieve and catalog lyrics for all of an artist's songs
and store their frequency counts by year.

Take a look at [word_count_by_year](https://github.com/unscsprt/lyrics-analysis/blob/master/analysis.py#L42)
to get an idea of how that works under the hood.

In a nutshell, that left me with data in this form:

{% highlight json %}
{
  '1988': {
    'spot': 3,
    'the': 32,
    'dog': 4
  },
  '1993': {
    'ran': 4,
    'the': 12,
    'track': 2
  }
}
{% endhighlight %}

## Step 3) Store results in an easily accessible format.

CSV to the rescue! I elected to write a CSV file for each year included in
the results. Each row of these files looked like so:

{% highlight text %}
# Ex - 1988.csv

hamster, 13
dog, 4
guitar, 5
{% endhighlight %}

After all is said and done, we can end up with a folder full of these CSV files
that we can open in programs like Excel for further analysis. Just from these
results, we can easily sort by word count and see the most popular words
used by an artist in any given year.

# Going forward...

Obviously, this data will yield more interesting results after I find an
efficient way of aggregating all of the separate years, but like I mentioned
earlier, the possibilities are many.

Some next steps will be improving the accuracy of excluding duplicate albums
retrieved from MusixMatch and adding additional output file formats for
more powerful analysis.

My next update will include actual findings that I get from the data.

Feel free to play around with it; you'll find the repo [here on GitHub](https://github.com/unscsprt/lyrics-analysis/blob/master/analysis.py#L42).
