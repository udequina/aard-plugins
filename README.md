
Aard Inventory Manager

This plugin manages your inventory and gives you tools to analyze and use
items in your inventory.

The first step you'll need to take is to find an out-of-the-way room where
you won't disturb anyone and then run "dinv build confirm".  This step
will identify all items you are wearing, all items in your main inventory,
and all items in containers in your inventory.  This will take roughly 5
minutes depending on how many items are in your inventory.  If you need to
halt the build simply go to sleep or go AFK.  You can "refresh" your
inventory later to complete the process of identifying everything you are
carrying.

Once you have a completed inventory table available, I recommend making a
manual backup in case something goes wrong in the future.  You can restore
from the backup and avoid the long build process again.  If anything in
your inventory has changed since the backup, your next refresh will simply 
update it to what you currently have.  To make a manual backup named
"my_first_awesome_backup" type "dinv backup create my_first_awesome_backup".
See "dinv help backup" for more details about creating, viewing, and restoring
backups.

Type "dinv help" for all available usage options.  For detailed examples of
how to use each option, simply type "dinv help <option>".


Usage
=====

  Inventory table access
    dinv build [confirm]
    dinv refresh [on | off | all] <minutes>
    dinv search [id | full] <query>

  Item management
    dinv get <query>
    dinv put <container relative name> <query>
    dinv store <query>
    dinv keyword [add | remove] <keyword name> <query>
    dinv organize [add | clear | display] <container relative name> <query>

  Equipment sets
    dinv set [display | wear] <priority name> <level>
    dinv snapshot [create | delete | list | display | wear] <snapshot name>
    dinv priority [list | display | compare] <priority name 1> <priority name 2>

  Equipment analysis
    dinv analyze [list | create | delete | display] <priority name> <positions>
    dinv usage <priority name | all> <query>
    dinv compare <priority name> <relative name>
    dinv covet <priority name> <auction #>

  Advanced options
    dinv backup [list | create | delete | restore] <backup name>
    dinv reset [list | confirm] <module names | all>
    dinv forget <query>
    dinv notify [none | light | standard | all]
    dinv cache [reset | size] [recent | frequent | all] <# entries>
    dinv tags <names | all> [on | off]

  Using equipment items
    dinv consume [add | remove | display | buy | small | big] <type> <name or quantity> <container>
    dinv portal [use] <portal object ID>
    dinv pass <pass ID> <# of seconds>

  Plugin info
    dinv version
    dinv help <command>


Release Notes
=============

1) There currently is no mechanism to change stat prioritization other than manually modifying the
   plugin.  This is high on the list of new features and a GUI supporting this will hopefully be
   coming in the future.

2) If you give an item to an enchanter to boost the item's stats, you may pull the old stats from a
   cache instead of using the new enchantments when you get the item back.  In this case, you may use
   the "dinv forget <query>" option to remove that item from your inventory table and its related caches.
   The next inventory refresh will pick up the new stats for the item.

3) Most aard operations that modify an item's stats are detected and automatically trigger a 
   re-identification.  For example, enchantment spells, sharpening, reinforcing, tpenchanting, and wset
   all are handled transparently.  The one known exception is the setweight command which is not
   currently handled by aard's invitem system.  Until this is changed (or until we include a trigger
   watching for setweight) you will need to use the "dinv forget <query>" option on an item that changes
   weight in order to "forget" the old stats and then pick up the correct weight (and other stats) on
   the next inventory refresh.

4) Wands and staves are not re-identified as they are used.  As a result, the number of charges
   shown in the item's display may not match reality as the item is used.  If this mode is not added
   to the aard invitem system, we may need to add a trigger to watch for this and update charges
   accordingly.

5) The plugin does not automatically open containers that are closed.  As a result, you won't be able
   to get/put items in a closed container.  Keep your containers open! :)

6) If you add a keyword to an item, drop it, and then pick it back up, the new keyword will still
   be available on the item if it is still in your cache.  However, this is not the case for common
   consumable items.  For example, we treat all duff beer potions as being identical so that you don't
   need to individually identify each potion.  As a result, the cached versions of those common items
   won't maintain custom keywords.

7) If the plugin tags are enabled, they will echo an end tag at the conclusion of an operation.  However,
   if the user goes into a state that doesn't allow echoing (e.g., AFK) then the plugin cannot report the
   end tag.  In this scenario, the plugin will notify the user about the end tag via a warning notification
   instead of an echo.  Triggers cannot catch notifications though so any code relying on end tags should
   either detect when you go AFK or cleanly time out after a reasonable amount of time.


Feature Wishlist
================

1) Add a way to create and update stat prioritizations.  A GUI would be a very convenient way
   to handle this.

2) Implement a mechanism to (more) fully identify items if the identify wish is not available.
   For example, we could use lore, the identify or object read spells, or Hester's identify
   service found at "runto identify".  This would be a manual process to "clean up" partially
   identified items.

3) Add a display option to de-duplicate potions and pills in search results
