Recently I made some updates for this websites. However, no changes will be visible since they all on the backend side :)

***

## Server Cache

I've set a simple cache mechanism on the server. Just like the cache between Higher level and Lower level of computer memory (Register => Main memory => Secondary memory), cache could be used on the web server to store some data temporarily from database, which leads to faster data fetching for clients. Of course the real mechanism of computer cache is much more fantastic because of the `Principle of Locality`. But still, have some variables on server to save the frequently visited data from the database do save the transferring time between database and server, also, it eases the possible overload of the database. The client will only need to wait for the server send them data directly instead of the server need to make another request to the database. 

Well, you could see that currently my website is really small-scale without many data to store, so the server cache could basically store almost all the information from the database once the server file starts to execute. Some data such as a list of data pointers may need to listen for the updates from the database (which could happen at any time), hence not suitable to be saved in cache. While the content of an article (which is pointed by a article pointer), could be read from the cache directly. Foe example, this article, which you are currently reading, might just be fetched from the cache.

***

## Markdown Format

Another update for this website is that the article content could be displayed through the markdown format now. Which is quite helpful for the developer. Before this update, what I did is a complicated way by using html code directly, then manually escape the HTML special characters and consider deal with the `\n` by using the block-displaying HTML tag or just a `<br/>` tag. Now, thanks to the `marked`, which is a package from NPM. I could store the article written by markdown grammar into the database directly. Also, thanks to the `Typora`, which is a really handy tool to write a markdown file.