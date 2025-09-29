|     |     |
| --- | --- |
| **Title:** | Google Earth |
| **Description:** | Official Google repo for Google Earth stable |
| **Daily Build:** | no  |

Setup key with:

`wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -`

Setup repository with:

`sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/earth/deb/ stable main" >> /etc/apt/sources.list.d/google.list'`

Setup package with:

`sudo apt-get update`
`sudo apt-get install <package name>`

where &lt;package name&gt; is the name of the package you want to install.