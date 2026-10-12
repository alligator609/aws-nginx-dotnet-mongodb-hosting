# MySQL Migration Guide: Large `.sql` Dump on Linux


## Install pv

```
sudo apt update
sudo apt install pv tmux -y

```

## Start a tmux session

```
tmux new -s precise-migration
```
## Run the import with percentage progress

```
 pv /var/www/backup/yourdatabase.sql | sudo mysql -u root -p yourdatabase

```
## With error handeling 

```
 pv -pterb /var/www/backup/yourdatabase.sql | \
mysql -u YOUR_MYSQL_USER -p --default-character-set=utf8mb4 yourdatabase \
2> /var/www/backup/yourdatabase.log

```

## Keep it running in the background

To detach from tmux without stopping the import, press:
- Ctrl+B
- Then D

## tmux list 

```
tmux ls
```
## Reconnect later:

```
tmux attach -t precise-migration
```