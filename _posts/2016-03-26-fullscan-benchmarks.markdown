---
layout: post
title: Benchmarking full scans 
date: 2016-03-26 11:00:00.000000000 -04:00
categories:
- tech
tags:
- databases
- mongo
- cassandra
type: post
---

With analytics it is common to perform large table scans as all the data needs to be considered. There are flat file options like Parquet but they come with pretty large restrictions. What are the database options like. Here I compare large reads from Mongo, Casandra, and CSV.

I will be running both Mongo and Cassandra from inside Docker. 

# Mongo

I've used no additional indexes beyond the default `_id`. There are 128454284 documents, and the collections stats are:

```
{
        "ns" : "bench_test.actions",
        "count" : 128454284,
        "size" : 83170724248,
        "avgObjSize" : 647,
        "storageSize" : 31919263744,
        "capped" : false,
        "wiredTiger" : {
                "metadata" : {
                        "formatVersion" : 1
                },
                "creationString" : "allocation_size=4KB,app_metadata=(formatVersion=1),block_allocation=best,block_compressor=snappy,cache_resident=0,checksum=on,colgroups=,collator=,columns=,dictionary=0,encryption=(keyid=,name=),exclusive=0,extractor=,format=btree,huffman_key
=,huffman_value=,immutable=0,internal_item_max=0,internal_key_max=0,internal_key_truncate=,internal_page_max=4KB,key_format=q,key_gap=10,leaf_item_max=0,leaf_key_max=0,leaf_page_max=32KB,leaf_value_max=64MB,log=(enabled=),lsm=(auto_throttle=,bloom=,bloom_bit_count=16,bloom_conf
ig=,bloom_hash_count=8,bloom_oldest=0,chunk_count_limit=0,chunk_max=5GB,chunk_size=10MB,merge_max=15,merge_min=0),memory_page_max=10m,os_cache_dirty_max=0,os_cache_max=0,prefix_compression=0,prefix_compression_min=4,source=,split_deepen_min_child=0,split_deepen_per_child=0,spli
t_pct=90,type=file,value_format=u",
                "type" : "file",
                "uri" : "statistics:table:collection-4--6983004796286940855",
                "LSM" : {
                        "bloom filters in the LSM tree" : 0,
                        "bloom filter false positives" : 0,
                        "bloom filter hits" : 0,
                        "bloom filter misses" : 0,
                        "bloom filter pages evicted from cache" : 0,
                        "bloom filter pages read into cache" : 0,
                        "total size of bloom filters" : 0,
                        "sleep for LSM checkpoint throttle" : 0,
                        "chunks in the LSM tree" : 0,
                        "highest merge generation in the LSM tree" : 0,
                        "queries that could have benefited from a Bloom filter that did not exist" : 0,
                        "sleep for LSM merge throttle" : 0
                },
                "block-manager" : {
                        "file allocation unit size" : 4096,
                        "blocks allocated" : 2644015,
                        "checkpoint size" : 31917375488,
                        "allocations requiring file extension" : 2640313,
                        "blocks freed" : 9264,
                        "file magic number" : 120897,
                        "file major version number" : 1,
                        "minor version number" : 0,
                        "file bytes available for reuse" : 2068480,
                        "file size in bytes" : 31919263744
                },
                "btree" : {
                        "btree checkpoint generation" : 74,
                        "column-store variable-size deleted values" : 0,
                        "column-store fixed-size leaf pages" : 0,
                        "column-store internal pages" : 0,
                        "column-store variable-size RLE encoded values" : 0,
                        "column-store variable-size leaf pages" : 0,
                        "pages rewritten by compaction" : 0,
                        "number of key/value pairs" : 0,
                        "fixed-record size" : 0,
                        "maximum tree depth" : 5,
                        "maximum internal page key size" : 368,
                        "maximum internal page size" : 4096,
                        "maximum leaf page key size" : 3276,
                        "maximum leaf page size" : 32768,
                        "maximum leaf page value size" : 67108864,
                        "overflow pages" : 0,
                        "row-store internal pages" : 0,
                        "row-store leaf pages" : 0
                },
                "cache" : {                                                                                                                                                                                                                                                   [4/1859]
                        "bytes read into cache" : 84523119969,
                        "bytes written from cache" : 84498646329,
                        "checkpoint blocked page eviction" : 0,
                        "unmodified pages evicted" : 2433644,
                        "page split during eviction deepened the tree" : 2,
                        "modified pages evicted" : 14260,
                        "data source pages selected for eviction unable to be evicted" : 995,
                        "hazard pointer blocked page eviction" : 480,
                        "internal pages evicted" : 6424,
                        "internal pages split during eviction" : 58,
                        "leaf pages split during eviction" : 11688,
                        "in-memory page splits" : 10943,
                        "in-memory page passed criteria to be split" : 24955,
                        "overflow values cached in memory" : 0,
                        "pages read into cache" : 2631923,
                        "pages read into cache requiring lookaside entries" : 0,
                        "overflow pages read into cache" : 0,
                        "pages written from cache" : 2643920,
                        "page written requiring lookaside records" : 0,
                        "pages written requiring in-memory restoration" : 0
                },
                "compression" : {
                        "raw compression call failed, no additional data available" : 0,
                        "raw compression call failed, additional data available" : 0,
                        "raw compression call succeeded" : 0,
                        "compressed pages read" : 2620214,
                        "compressed pages written" : 2618049,
                        "page written failed to compress" : 1,
                        "page written was too small to compress" : 25870
                },
                "cursor" : {
                        "create calls" : 6,
                        "insert calls" : 128454284,
                        "bulk-loaded cursor-insert calls" : 0,
                        "cursor-insert key and value bytes inserted" : 83796127832,
                        "next calls" : 129280601,
                        "prev calls" : 1,
                        "remove calls" : 0,
                        "cursor-remove key bytes removed" : 0,
                        "reset calls" : 129494236,
                        "restarted searches" : 17285696,
                        "search calls" : 0,
                        "search near calls" : 1020074,
                        "truncate calls" : 0,
                        "update calls" : 0,
                        "cursor-update value bytes updated" : 0
                },
                "reconciliation" : {
                        "dictionary matches" : 0,
                        "internal page multi-block writes" : 1576,
                        "leaf page multi-block writes" : 10976,
                        "maximum blocks required for a page" : 16,
                        "internal-page overflow keys" : 0,
                        "leaf-page overflow keys" : 0,
                        "overflow values written" : 0,
                        "pages deleted" : 28,
                        "fast-path pages deleted" : 0,
                        "page checksum matches" : 11655,
                        "page reconciliation calls" : 17577,
                        "page reconciliation calls for eviction" : 2569,
                        "leaf page key bytes discarded using prefix compression" : 0,
                        "internal page key bytes discarded using suffix compression" : 2621632
                },
                "session" : {
                        "object compaction" : 0,
                        "open cursor count" : 6
                },
                "transaction" : {
                        "update conflicts" : 0
                }
        },
                "cache" : {                                                                                                                                                                                                                                                   [4/1859]
                        "bytes read into cache" : 84523119969,
                        "bytes written from cache" : 84498646329,
                        "checkpoint blocked page eviction" : 0,
                        "unmodified pages evicted" : 2433644,
                        "page split during eviction deepened the tree" : 2,
                        "modified pages evicted" : 14260,
                        "data source pages selected for eviction unable to be evicted" : 995,
                        "hazard pointer blocked page eviction" : 480,
                        "internal pages evicted" : 6424,
                        "internal pages split during eviction" : 58,
                        "leaf pages split during eviction" : 11688,
                        "in-memory page splits" : 10943,
                        "in-memory page passed criteria to be split" : 24955,
                        "overflow values cached in memory" : 0,
                        "pages read into cache" : 2631923,
                        "pages read into cache requiring lookaside entries" : 0,
                        "overflow pages read into cache" : 0,
                        "pages written from cache" : 2643920,
                        "page written requiring lookaside records" : 0,
                        "pages written requiring in-memory restoration" : 0
                },
                "compression" : {
                        "raw compression call failed, no additional data available" : 0,
                        "raw compression call failed, additional data available" : 0,
                        "raw compression call succeeded" : 0,
                        "compressed pages read" : 2620214,
                        "compressed pages written" : 2618049,
                        "page written failed to compress" : 1,
                        "page written was too small to compress" : 25870
                },
                "cursor" : {
                        "create calls" : 6,
                        "insert calls" : 128454284,
                        "bulk-loaded cursor-insert calls" : 0,
                        "cursor-insert key and value bytes inserted" : 83796127832,
                        "next calls" : 129280601,
                        "prev calls" : 1,
                        "remove calls" : 0,
                        "cursor-remove key bytes removed" : 0,
                        "reset calls" : 129494236,
                        "restarted searches" : 17285696,
                        "search calls" : 0,
                        "search near calls" : 1020074,
                        "truncate calls" : 0,
                        "update calls" : 0,
                        "cursor-update value bytes updated" : 0
                },
                "reconciliation" : {
                        "dictionary matches" : 0,
                        "internal page multi-block writes" : 1576,
                        "leaf page multi-block writes" : 10976,
                        "maximum blocks required for a page" : 16,
                        "internal-page overflow keys" : 0,
                        "leaf-page overflow keys" : 0,
                        "overflow values written" : 0,
                        "pages deleted" : 28,
                        "fast-path pages deleted" : 0,
                        "page checksum matches" : 11655,
                        "page reconciliation calls" : 17577,
                        "page reconciliation calls for eviction" : 2569,
                        "leaf page key bytes discarded using prefix compression" : 0,
                        "internal page key bytes discarded using suffix compression" : 2621632
                },
                "session" : {
                        "object compaction" : 0,
                        "open cursor count" : 6
                },
                "transaction" : {
                        "update conflicts" : 0
                }
        },
                "cache" : {                                                                                                                                                                                                                                                   [4/1859]
                        "bytes read into cache" : 84523119969,
                        "bytes written from cache" : 84498646329,
                        "checkpoint blocked page eviction" : 0,
                        "unmodified pages evicted" : 2433644,
                        "page split during eviction deepened the tree" : 2,
                        "modified pages evicted" : 14260,
                        "data source pages selected for eviction unable to be evicted" : 995,
                        "hazard pointer blocked page eviction" : 480,
                        "internal pages evicted" : 6424,
                        "internal pages split during eviction" : 58,
                        "leaf pages split during eviction" : 11688,
                        "in-memory page splits" : 10943,
                        "in-memory page passed criteria to be split" : 24955,
                        "overflow values cached in memory" : 0,
                        "pages read into cache" : 2631923,
                        "pages read into cache requiring lookaside entries" : 0,
                        "overflow pages read into cache" : 0,
                        "pages written from cache" : 2643920,
                        "page written requiring lookaside records" : 0,
                        "pages written requiring in-memory restoration" : 0
                },
                "compression" : {
                        "raw compression call failed, no additional data available" : 0,
                        "raw compression call failed, additional data available" : 0,
                        "raw compression call succeeded" : 0,
                        "compressed pages read" : 2620214,
                        "compressed pages written" : 2618049,
                        "page written failed to compress" : 1,
                        "page written was too small to compress" : 25870
                },
                "cursor" : {
                        "create calls" : 6,
                        "insert calls" : 128454284,
                        "bulk-loaded cursor-insert calls" : 0,
                        "cursor-insert key and value bytes inserted" : 83796127832,
                        "next calls" : 129280601,
                        "prev calls" : 1,
                        "remove calls" : 0,
                        "cursor-remove key bytes removed" : 0,
                        "reset calls" : 129494236,
                        "restarted searches" : 17285696,
                        "search calls" : 0,
                        "search near calls" : 1020074,
                        "truncate calls" : 0,
                        "update calls" : 0,
                        "cursor-update value bytes updated" : 0
                },
                "reconciliation" : {
                        "dictionary matches" : 0,
                        "internal page multi-block writes" : 1576,
                        "leaf page multi-block writes" : 10976,
                        "maximum blocks required for a page" : 16,
                        "internal-page overflow keys" : 0,
                        "leaf-page overflow keys" : 0,
                        "overflow values written" : 0,
                        "pages deleted" : 28,
                        "fast-path pages deleted" : 0,
                        "page checksum matches" : 11655,
                        "page reconciliation calls" : 17577,
                        "page reconciliation calls for eviction" : 2569,
                        "leaf page key bytes discarded using prefix compression" : 0,
                        "internal page key bytes discarded using suffix compression" : 2621632
                },
                "session" : {
                        "object compaction" : 0,
                        "open cursor count" : 6
                },
                "transaction" : {
                        "update conflicts" : 0
                }
        },
                "cache" : {                                                                                                                                                                                                                                                   [4/1859]
                        "bytes read into cache" : 84523119969,
                        "bytes written from cache" : 84498646329,
                        "checkpoint blocked page eviction" : 0,
                        "unmodified pages evicted" : 2433644,
                        "page split during eviction deepened the tree" : 2,
                        "modified pages evicted" : 14260,
                        "data source pages selected for eviction unable to be evicted" : 995,
                        "hazard pointer blocked page eviction" : 480,
                        "internal pages evicted" : 6424,
                        "internal pages split during eviction" : 58,
                        "leaf pages split during eviction" : 11688,
                        "in-memory page splits" : 10943,
                        "in-memory page passed criteria to be split" : 24955,
                        "overflow values cached in memory" : 0,
                        "pages read into cache" : 2631923,
                        "pages read into cache requiring lookaside entries" : 0,
                        "overflow pages read into cache" : 0,
                        "pages written from cache" : 2643920,
                        "page written requiring lookaside records" : 0,
                        "pages written requiring in-memory restoration" : 0
                },
                "compression" : {
                        "raw compression call failed, no additional data available" : 0,
                        "raw compression call failed, additional data available" : 0,
                        "raw compression call succeeded" : 0,
                        "compressed pages read" : 2620214,
                        "compressed pages written" : 2618049,
                        "page written failed to compress" : 1,
                        "page written was too small to compress" : 25870
                },
                "cursor" : {
                        "create calls" : 6,
                        "insert calls" : 128454284,
                        "bulk-loaded cursor-insert calls" : 0,
                        "cursor-insert key and value bytes inserted" : 83796127832,
                        "next calls" : 129280601,
                        "prev calls" : 1,
                        "remove calls" : 0,
                        "cursor-remove key bytes removed" : 0,
                        "reset calls" : 129494236,
                        "restarted searches" : 17285696,
                        "search calls" : 0,
                        "search near calls" : 1020074,
                        "truncate calls" : 0,
                        "update calls" : 0,
                        "cursor-update value bytes updated" : 0
                },
                "reconciliation" : {
                        "dictionary matches" : 0,
                        "internal page multi-block writes" : 1576,
                        "leaf page multi-block writes" : 10976,
                        "maximum blocks required for a page" : 16,
                        "internal-page overflow keys" : 0,
                        "leaf-page overflow keys" : 0,
                        "overflow values written" : 0,
                        "pages deleted" : 28,
                        "fast-path pages deleted" : 0,
                        "page checksum matches" : 11655,
                        "page reconciliation calls" : 17577,
                        "page reconciliation calls for eviction" : 2569,
                        "leaf page key bytes discarded using prefix compression" : 0,
                        "internal page key bytes discarded using suffix compression" : 2621632
                },
                "session" : {
                        "object compaction" : 0,
                        "open cursor count" : 6
                },
                "transaction" : {
                        "update conflicts" : 0
                }
        },
        "nindexes" : 1,
        "totalIndexSize" : 1303175168,
        "indexSizes" : {
                "_id_" : 1303175168
        },
        "ok" : 1
}
```


## Basic Read

Here is the script I used, very basic:

```python
from pymongo import MongoClient
actions = MongoClient()['bench_test']['actions'].find()
count = 0
print("Starting")
for i in actions:
    if count % 100000 == 0:
        print(count)
    count += 1
print("Done")
```

Execution #1
```
real    35m37.049s
user    25m22.424s
sys     1m44.028s
```

Execution #2
```
real    27m43.629s
user    18m40.636s
sys     1m33.896s
```

I did notice that python process was taking about 80% CPU which gives me some worry about Python being the bottleneck. Mongo around 20%. DiskIO is pretty fine at 10% and disk read throughput at 13.3M/s.



## Projection
But I know there is a lot of extra information in the documents. What if I wanted to isolate just 'customer-id' and 'timestamp'

### Projection - script
```python
from pymongo import MongoClient
p = {'timestamp': 1, 'customer-id': 1, '_id': 0}
actions = MongoClient()['bench_test']['actions'].find({}, p)
count = 0
print("Starting")
for i in actions:
    if count % 100000 == 0:
        print(count)
    count += 1
print("Done")
```

### Projection - Run #1
real    11m0.640s
user    3m17.836s
sys     0m25.832s


### Projection - Run #2
real    10m57.515s
user    3m16.616s
sys     0m25.736s




## Timestamp restriction
I added an index on `{'timestamp': 1}`. This yielded 91,080,438 documents.

### Timestamp restriction - script:
```python
from datetime import datetime
from pymongo import MongoClient
q = {'timestamp': 
    {'$gte': datetime(2015, 01, 01),
    '$lt': datetime(2016, 01, 01)}
}
actions = MongoClient()['bench_test']['actions'].find(q)
count = 0
print("Starting")
for i in actions:
    if count % 100000 == 0:
        print(count)
    count += 1
print("Total")
print(count)
print("Done")
```


### Timestamp resctriction - Run #1
```
real    23m43.569s
user    13m39.052s
sys     0m49.560s
```


### Timestamp resctriction - Run #2
```
Total
Done

real    23m54.677s
user    13m34.628s
sys     0m57.328s
```


# Copy time
```
real    38m19.203s
user    18m26.760s
sys     0m49.268s
```






What if, I didn't just supply a projection, but only stored the data I was interested in. I re-wrote out the collection so that I was only storing 'customer-id' and 'timestamp'.


Run #1
real    8m39.873s
user    5m27.732s
sys     1m0.084s

Run #2
real    7m42.522s
user    5m39.204s
sys     1m12.464s



Same data set now with index on `{'timestamp': 1}` and time restriction:


Run #1
real    9m7.167s
user    6m19.084s
sys     0m35.272s


Run #2
real    6m3.538s
user    4m12.300s
sys     0m25.560s
