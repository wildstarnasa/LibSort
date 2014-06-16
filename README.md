#LibSort 1.0

See the [Wiki](https://github.com/wildstarnasa/LibSort/wiki) for examples

##Rationale

Carbine have nicely given us the ability to sort the inventory of a BagWindow by passing through a function that takes two item arguments and returns an order (-1, 0 1). Currently this has been used in the Stock UI to sort the inventory by a single factor, be it Alphabetical, Quality, etc.

However, this is not using the full capacity available to a sort function of this calibre. Instead of a single data point to sort on, most of the inventory is complicated enough that something like Alphabetical is simply insufficient to order the items in a manner that's actually usable.

Instead, what is needed is to chain sort functions together, so that when an equality exists between two individual items, a further datapoint can be used to differentiate it. EG, you are sorting by Inventory Slot, but you have two items that fit in that slot. So, now you have to determine if one is perhaps higher level than the other so as to order them in a logical manner that's obvious to the player.

Enter LibSort. It's a port from a similar library in ESO where we had to do a whole lot more work to get it going, but it is essentially a library you can add to your addon to assist you in creating a sort chain that will suit your needs.

You provide the functions that do the datapoint matching, and tell the library what order to put them in, and the library will give you a single function you can feed into the Carbine SetItemSortComparer for the BagWindow.
