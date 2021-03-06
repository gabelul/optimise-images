[optimise-images.sh](https://github.com/centminmod/optimise-images) added new `profilelog` option to only log the image directory and profile output to a log file. You can then script it to parse through this profile log to process images. For example running a parallel script routine to process images listed in profile log etc.

Usage options:

    ./optimise-images.sh 
    ./optimise-images.sh {optimise} /PATH/TO/DIRECTORY/WITH/IMAGES
    ./optimise-images.sh {profile} /PATH/TO/DIRECTORY/WITH/IMAGES
    ./optimise-images.sh {profilelog} /PATH/TO/DIRECTORY/WITH/IMAGES
    ./optimise-images.sh {testfiles} /PATH/TO/DIRECTORY/WITH/IMAGES
    ./optimise-images.sh {install}
    ./optimise-images.sh {bench}
    ./optimise-images.sh {bench-compare}
    ./optimise-images.sh {bench-webp}
    ./optimise-images.sh {bench-webpcompare}

Full example of `profilelog` command mode where the columns refer to

```
image name : width : height : quality : transparency : image depth (bits) : size : user: group
```

    /root/tools/optimise-images/optimise-images.sh profilelog /home/nginx/domains/domain.com/public/images
    directory : /home/nginx/domains/domain.com/public/images
    image : bees.png : 444 : 258 : 92 : False : 8 : 175296 : root : nginx
    image : dslr_canon_eos_m6_1.jpg : 1200 : 800 : 82 : False : 8 : 161086 : root : nginx
    image : png24-image1.png : 600 : 400 : 92 : False : 8 : 386063 : root : nginx
    image : png24-interlaced-image1.png : 600 : 400 : 92 : False : 8 : 386063 : root : nginx
    image : samsung_s7_mobile_1.jpg : 2048 : 1536 : 82 : False : 8 : 256253 : root : nginx
    image : webp-study-source-firebreathing.png : 1024 : 752 : 92 : False : 8 : 1194091 : root : nginx
    
    logged at /home/optimise-logs/profile-log-280417-044228.log

    Completion Time: 0.08 seconds
    ------------------------------------------------------------------------------

Contents of generated profile log at `/home/optimise-logs/profile-log-280417-044228.log`

    cat /home/optimise-logs/profile-log-280417-044228.log

    directory : /home/nginx/domains/domain.com/public/images
    image : bees.png : 444 : 258 : 92 : False : 8 : 175296 : root : nginx
    image : dslr_canon_eos_m6_1.jpg : 1200 : 800 : 82 : False : 8 : 161086 : root : nginx
    image : png24-image1.png : 600 : 400 : 92 : False : 8 : 386063 : root : nginx
    image : png24-interlaced-image1.png : 600 : 400 : 92 : False : 8 : 386063 : root : nginx
    image : samsung_s7_mobile_1.jpg : 2048 : 1536 : 82 : False : 8 : 256253 : root : nginx
    image : webp-study-source-firebreathing.png : 1024 : 752 : 92 : False : 8 : 1194091 : root : nginx

Manipulate the profile log as you see fit

```
awk 'NR==1' /home/optimise-logs/profile-log-280417-044228.log          
directory : /home/nginx/domains/domain.com/public/images
```

```
awk -F " : " 'NR==1 {print $2}' /home/optimise-logs/profile-log-280417-044228.log
/home/nginx/domains/domain.com/public/images
```

```
awk '/image : /' /home/optimise-logs/profile-log-280417-044228.log
image : bees.png : 444 : 258 : 92 : False : 8 : 175296 : root : nginx
image : dslr_canon_eos_m6_1.jpg : 1200 : 800 : 82 : False : 8 : 161086 : root : nginx
image : png24-image1.png : 600 : 400 : 92 : False : 8 : 386063 : root : nginx
image : png24-interlaced-image1.png : 600 : 400 : 92 : False : 8 : 386063 : root : nginx
image : samsung_s7_mobile_1.jpg : 2048 : 1536 : 82 : False : 8 : 256253 : root : nginx
image : webp-study-source-firebreathing.png : 1024 : 752 : 92 : False : 8 : 1194091 : root : nginx
```

```
awk -F " : " 'NR>1 {print $2}' /home/optimise-logs/profile-log-280417-044228.log 
bees.png
dslr_canon_eos_m6_1.jpg
png24-image1.png
png24-interlaced-image1.png
samsung_s7_mobile_1.jpg
webp-study-source-firebreathing.png
```

Example while loop

```
DIR=$(awk -F " : " 'NR==1 {print $2}' /home/optimise-logs/profile-log-280417-044228.log)

awk -F " : " 'NR>1 {print $2}' /home/optimise-logs/profile-log-280417-044228.log | while read i; do
 echo "image name: ${DIR}/${i}"
done
```

will output

```
image name: /home/nginx/domains/domain.com/public/images/bees.png
image name: /home/nginx/domains/domain.com/public/images/dslr_canon_eos_m6_1.jpg
image name: /home/nginx/domains/domain.com/public/images/png24-image1.png
image name: /home/nginx/domains/domain.com/public/images/png24-interlaced-image1.png
image name: /home/nginx/domains/domain.com/public/images/samsung_s7_mobile_1.jpg
image name: /home/nginx/domains/domain.com/public/images/webp-study-source-firebreathing.png
```