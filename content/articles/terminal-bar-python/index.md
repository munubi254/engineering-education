### Introduction

Maybe your application involves a file upload or download at some point through the terminal/console. You will, at one point, want to show the progress in the said interface. Terminal progress bars are a great way to visualize to the user on the progress of an underlying task. We will look at how to do that using file downloads.

There are various libraries to do that in python including tqdm, Progress, Alive Progress, PyQt5 etc.


#### Prerequisites

An understanding of Python

> Since what we are going to wok is linked to the terminal, we will not be using any notebook-based technology.

#### A primer on file downloads in Python
Before we get into the topic, we will first look at a very basic download processing script in Python. We will use a simple script to download an image.

```python
import urllib.request

def Handle_Progress(block_num, block_size, total_size):
        read_data= 0
        ## calculate the progress
        temp = block_num * block_size
        read_data = temp + read_data
        remaining_size = total_size - read_data
        if(remaining_size<=0):
            downloaded_percentage = 100
            remaining_size = 0
        else:
            downloaded_percentage = ((total_size-remaining_size) / total_size)*(100)
              
        print(read_data," : ", remaining_size," : ", downloaded_percentage," : ", total_size)
            
def Download_File():
        download_url = 'https://sacco.terrence-aluda.com/sacco/images/ab1.jpg'
        site = urllib.request.urlopen(download_url)
        meta = site.info()
        print("File size in bytes", meta.get("Content-Length"))
        save_location = 'thispic.png'
        urllib.request.urlretrieve(download_url,save_location, Handle_Progress)
        
Download_File()

```

We first import `urllib.request` module used for processing URL requests.

```python
def Download_File():
        download_url = 'https://sacco.terrence-aluda.com/sacco/images/ab1.jpg'
        site = urllib.request.urlopen(download_url)
        meta = site.info()
        print("File size in bytes", meta.get("Content-Length"))
        save_location = 'thispic.png'
        urllib.request.urlretrieve(download_url,save_location, Handle_Progress)
```

The `Download_File()` function first gets the size of the file in bytes from the file's meta information response header, *Content-Length*. That is done after getting the file's contents using the `urlopen()` method of the imported module.

Thereafter, it starts downloading the file using the `urlretrieve()` method. THis method takes in three parameters which I'll explain their usage below.
1. `download_url` - The URL where the file to be downloaded is located.
2. `save_location` - The location where the downloaded file will be stored.
3. `Handle_Progress` - THe function for processing the download progress, and it is passed as a callback. We'll look at this function in the next part.

```python
def Handle_Progress(block_num, block_size, total_size):
        read_data= 0
        ## calculate the progress
        temp = block_num * block_size
        read_data = temp + read_data
        remaining_size = total_size - read_data
        if(remaining_size<=0):
            downloaded_percentage = 100
            remaining_size = 0
        else:
            downloaded_percentage = ((total_size-remaining_size) / total_size)*(100)
              
        print(read_data," : ", remaining_size," : ", downloaded_percentage," : ", total_size)
```

This function takes three parameters:
1. `block_num` - THe block number. The methd gets the file in  blocks.
2. `block_size` - The size in bytes of the block.
3. `total_size` - The total size in bites of the file.

Using these three parameters, it calculates the  the downloaded size(`read_data`), remaining size(`remaining_size`), and downloaded percentage(`downloaded_percentage`).
 > It gets the downloaded size by calculating the bloack number multiplied by the block size, then repeatedly adds it till the file ifs complete.

The `if` function checks for the remaining size. If it is less thsan 0, then it means the file is fully downloaded. Therefore, the downloaded percentage is 100% and the remaining size is 0. The remaining size can be negative since the block sizes are constant.

On running the file, you should see this output. It's not so pretty, but it just gives you an idea of the progress.