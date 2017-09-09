Initial import

sudo docker build -t pinpoint-hippo .

sudo docker run -i -t -p 28080-28082:8080-8082 --cap-add SYS_PTRACE pinpoint-hippo

hbase/hbase-1.0.3/bin/start-hbase.sh

hbase/hbase-1.0.3/bin/hbase shell pinpoint/hbase/scripts/hbase-create.hbase 