#LibSort 1.0

See the [Wiki](https://github.com/wildstarnasa/LibSort/wiki) for examples

**Table of Contents**  
- [Rationale](#user-content-rationale)
    - [API](#user-content-api)
        - [Register](#user-content-register)
        - [RegisterDefaultOrder](#user-content-registerdefaultorder)

##Rationale

Carbine have nicely given us the ability to sort the inventory of a BagWindow by passing through a function that takes two item arguments and returns an order (-1, 0 1). Currently this has been used in the Stock UI to sort the inventory by a single factor, be it Alphabetical, Quality, etc.

However, this is not using the full capacity available to a sort function of this calibre. Instead of a single data point to sort on, most of the inventory is complicated enough that something like Alphabetical is simply insufficient to order the items in a manner that's actually usable.

Instead, what is needed is to chain sort functions together, so that when an equality exists between two individual items, a further datapoint can be used to differentiate it. EG, you are sorting by Inventory Slot, but you have two items that fit in that slot. So, now you have to determine if one is perhaps higher level than the other so as to order them in a logical manner that's obvious to the player.

Enter LibSort. It's a port from a similar library in ESO where we had to do a whole lot more work to get it going, but it is essentially a library you can add to your addon to assist you in creating a sort chain that will suit your needs.

You provide the functions that do the datapoint matching, and tell the library what order to put them in, and the library will give you a single function you can feed into the Carbine SetItemSortComparer for the BagWindow.

---
##API

###Register
This will register a Datapoint function
    
    LibSort:Register(addonName, name, desc, key, func)

- *addonName* - The name of the registering addon 
    + Example: "Item Sort"
- *name* - A unique registration name 
    + Example: "ISWeaponSort"
- *desc* - A description of how the sort applies 
    + Example: "Will sort by Weapon Type"
- *key* - A unique key used to identify the datapoint
    + Example: "weaponType"
- *func* - The function to call to retrieve the sort value. Function signature **needs** to be (itemLeft, itemRight)
    + Example: ItemSort.WeaponSort

###RegisterDefaultOrder
Your addon may have multiple registrations, and this function will allow you to indicate what order you want them in as a block. Call this function *after* you have completed your registrations

There are two tables you can pass in, for *low level* and *high level* keys. 

- Low level keys are values that are unique to certain types of items, like ItemCategory. 
- High level keys are those linked to values that are common across larger swathes of items, like item level, or name. 
 

If you separate your keys in the two tables, LibSort will first chain all the low level keys before all high level keys, so that multiple addons can apply sort orders without getting cut off. (It's highly recommended that you split keys if you use high level definitions) 

This is somewhat of a holdover from the ESO edition, but if you're adding to another addon's sorting, this will be useful.

Default behaviour, by not using this API call will be order of registration at a high level to avoid breaking other registrations, and thus may not work as you expect, so make sure you set it.

    LibSort:RegisterDefaultOrder(addonName, keyTableLow, keyTableHigh)

- *addonName* -The name of the registering addon
    + Example: "Item Sort"
- *keyTableLow* - A table indicating the order of low level sortKeys for this addon
    + Example: {"weaponType", "armorEquipType", "armorType"}
- *keyTableHigh* - **Optional** A table indicating of the order of high level sortKeys for this addon
    + Example: {"subjectiveItemLevel"}
