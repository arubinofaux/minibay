Accessible music.

License
=======
Copyright (C) 2013 Dionysis "dionyziz" Zindros <dionyziz@gmail.com>

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

Installation
============

To install this on your own server, follow these steps. I have included the particular versions I am using that I have verified to work.

 * Install these on your Debian server:
  * Apache 2.2.16
  * MySQL
  * Python 2.6.6
  * git
  * transmission-daemon
  * p7zip, p7zip-full
 * Create a local directory structure. I recommend the following structure:
  * /home/minibay/downloads - To store the downloads
  * /home/minibay/minibay-django - To store the django project files (this is where you should clone)
  * /var/www/yourdomain.example.com/static - To store your static files
 * [Set up transmission-daemon](http://www.webupd8.org/2009/12/setting-up-transmission-remote-gui-in.html) with a username and a password and make it run on system startup.
 * Verify that transmission is installed correctly by connecting to it using Transmission Remote GUI.
 * In your virtualenv or globally, install these python modules:
  * BitTorrent-bencode==5.0.8.1
  * Django==1.5
  * MySQL-python==1.2.2
  * South==0.7.6
  * Unidecode==0.04.12
  * progressbar==2.3
  * transmissionrpc==0.9
  * wsgiref==0.1.2
 * Install the following Apache modules:
  * mod_wsgi
  * mod_xsendfile
 * git clone [this repository](https://github.com/dionyziz/minibay) into /home/minibay/minibay-django
 * Create a MySQL database and a user for it, with a username and a password and full permissions on the database.
 * Create the file settings-local.py next to settings.py in the folder minibay-django/minibay and include:
  * Your MySQL username and password
  * Your local downloads directory
  * Your transmission credentials
 * Change your MySQL configuration file to support the fine-tuning options for fulltext search from the file minibay/my.cfg. Restart MySQL.
 * Build the database using `manage.py syncdb`. You have to do this after modifying my.cfg!
 * [Enable south on the django app](http://south.readthedocs.org/en/latest/convertinganapp.html#converting-an-app)
 * Make sure your tables use the MyISAM engine.
 * Create a FULLTEXT index on the column bay_files.name by manually running South migration #0004 or by issuing the appropriate SQL query.
 * Configure apache. A sample configuration file is provided at minibay/apache.conf.
 * Download The Pirate Base database from the following archives onto your server:

  * [The Pirate Bay 3200000 - 7700000](https://thepiratebay.se/torrent/7706886/Backup_of_The_Pirate_Bay_(IDs__3200000_-_7700000))
  * [The Pirate Bay 7700000 - 7999999](https://thepiratebay.se/torrent/8044295/Backup_of_The_Pirate_Bay_(IDs__7700000_-_7999999)_)
  * If you want more recent torrents available [git clone tpb2csv and export Pirate Bay to csv manually](https://github.com/andronikov/tpb2csv)
 * If you downloaded one of the archives, you should have a folder with .7z files. To import, run the following command in it, after verifying that directory structure is correct by looking at the source code of the .sh file, to import. Importing can take a couple of days, so you may want to run it under `screen`.

         /home/minibay/minibay-django/scripts/import-tpb.sh
 
 * Otherwise, if you just have plain directories that you have manually exported, directly run `manage.py import-tbp` with the appropriate arguments in a for loop.
 * You can now get rid of the .7z and .csv files, as everything is in MySQL.
