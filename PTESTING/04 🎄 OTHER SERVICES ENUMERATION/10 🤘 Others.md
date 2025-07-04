Mongo DB port 27017:
```sh
mongo mongodb://10.129.189.148:27017
```

MONGO enumeration:
```
show dbs;
use $DB
show collections
db.$COLLECTION.find().pretty()
```

RSYNC:
```sh
rsync --list-only $TARGET_IP::
rsync --list-only $TARGET_IP::$DIR
rsync $TARGET_IP::$DIR/$FILE $FILE
```

Send email using SMTP server and swaks:
```bash
swaks -t $VICTIM@$TARGET_AD --from $USER@$TARGET_AD -ap --attach config.Library-ms --server $SMTP_SERVER --body body.txt --header "Subject: Interesting" --suppress-data
```

MQTT:

```bash
nmap --script mqtt-subscribe -p 1883,8883 $TARGET_IP
```
