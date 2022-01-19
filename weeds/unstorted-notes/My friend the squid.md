# My friend the squid

## Need
I was a Scrappy little IT technician. My first experience with harddrives was daisy chaining 5 or so SCSI Drives out of the back of a crappy little 266mhz machine that I found on the side of the road. I ended up bolting them to a piece of cardboard and taping to the side of the computer.

I bought my first new hard-drive when I was still in high-school. At the time when SATA was just taking off, and there was a lot of talk in PC mag of how ATA 133 was just plain silly, I snagged an 80gb Western Digital WD800JB for a little less than $80.  It was a pretty big step up from my [gateway essential 433c][1]'s little ~12gb drive.

The same model now [retails for $9.99][2] if you can even find it anymore. Talk about commodity hardware.

Over the years, my collection of drives has waxed and warned. I would add to my collection in piecemeal manner - snagging a drive from a friend headed for the bin, or saving up and dropping another $100 here or there when the price was right. My first serious foray into using excel for practical purposes was to chart price/GB on sites like directron.com.

As I grew older I was able to invest in external drives. Starting at a not disrespectful 250gb drive in an external enclosure and clone if it shortly here-after. Sadly those would be lost due to physical failures with the power supply. By the time I was out of college, I had amassed a total of 2tbs of space between the internal drives and a pretty sizable 500gb  My total storage space has steadily increased to a point where I can boast a sizable 10tb of effective space among 4 computers (if you include my wife's).

~ 9tb of hard-drive space

x internals
    1tb
    1tb
    640gb
    320gb
x externals

## Background

About Ceph: [Fundamentals of Ceph by Greg Farnum][3] 

UC Santa Cruz

Scalable - It is designed to run on petabytes and exabytes

Distributed - designed to run on lots of servers, how every many you care together, on a local area network.

Software based - You take whatever hardware you want, and you run the Ceph storage system daemons on it, and they aggregate into a single unified Ceph cluster.

Designed for Failure - Ceph deals with hardware failures natively. It replicates all the data in the system according to policies that you set. and then when something fails it automatically re-replicates that data elsewhere to provide the durability guarantees that you have asked for.

Open Source from its onset - Not a one-off project - Non Proprietary - 3rd Party Vendor Support - Kernel Support

  [1]: http://www.pcauthority.com.au/Review/16163,gateway-essential-433.aspx
  [2]: http://www.directron.com/wd800bbnp.html
  [3]: https://www.youtube.com/watch?v=lG6eeUNw9iI