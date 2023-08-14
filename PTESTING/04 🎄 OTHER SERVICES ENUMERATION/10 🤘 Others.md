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
