CraftBukkit-MCN-sirati97-Fork named OilSpigot
======
An implementation of the [Bukkit](https://hub.spigotmc.org/stash/projects/SPIGOT/repos/bukkit) plugin API for [Minecraft](https://minecraft.net/) servers, currently maintained by [SpigotMC](http://www.spigotmc.org/).

As well as an implementation of the [OilMod](https://github.com/OilMod/OldMod-API) server-side modding API for [Minecraft](https://minecraft.net/) servers, currently maintained by [sirati97](http://t.me/sirati97/).

SO far this file is merely a copy of Spigots and not all text is adjusted correctly. 

Bug Reporting
-------------
The development team is very open to both bug and feature requests / suggestions. You can submit these on the [JIRA Issue Tracker](http://hub.spigotmc.org/jira/).

Compilation
-----------
CraftBukkit is a Java program which uses [Maven 3](http://maven.apache.org/) for compilation. To compile fresh from Git, simply perform the following steps:

* Install Git using your preferred installation methods.
* Download and run [BuildTools](https://www.spigotmc.org/wiki/buildtools/)

Some IDEs such as [IntelliJ IDEA](https://www.jetbrains.com/idea/) can perform these steps for you. Any Maven capable Java IDE can be used to develop with OilSpigot, however the current team's personal preference is to use IntelliJ IDEA.

Contributing
------------
Contributions of all sorts are welcome. To manage community contributions, we use the pull request functionality of Stash. In to gain access to Stash and create a pull request, you will first need to perform the following steps:

* Create an account on [JIRA](http://hub.spigotmc.org/jira/).
* Fill in the [SpigotMC CLA](http://www.spigotmc.org/go/cla) and wait up to 24 hours for your Stash account to be activated. Please ensure that your username and email addresses match.
* Log into Stash using your JIRA credentials.

Once you have performed these steps you can create a fork, push your code changes, and then submit it for review.

If you submit a PR involving both OilMod and OilSpigot, each PR should link the other.

Although the minimum requirement for compilation & usage is Java 8, we prefer all contributions to be written in Java 7 style code unless there is a compelling reason otherwise.

Code Requirements
-----------------
* For the most part, OilSpigot and OilMod use the [Sun/Oracle coding standards](http://www.oracle.com/technetwork/java/javase/documentation/codeconvtoc-136057.html).
* No tabs; use 4 spaces instead.
    * Empty lines should contain no spaces.
* No trailing whitespaces.
* No 80 character column limit, or 'weird' mid-statement newlines unless absolutely necessary.
    * The 80 character column limit still applies to documentation.
* No one-line methods.
* All major additions should have documentation.
* Try to follow test driven development where available.
* All code should be free of magic values. If this is not possible, it should be marked with a TODO comment indicating it should be addressed in the future.
  * If magic values are absolutely necessary for your change, what those values represent should be documented in the code as well as an explanation in the Pull Request description on why those values are necessary.
* No unnecessary code changes. Look through all your changes before you submit it.
* Do not attempt to fix multiple problems with a single patch or pull request.
* Do not submit your personal changes to configuration files.
* Avoid moving or renaming classes.

OilMod/OilSpigot employs [JUnit 4](http://www.vogella.com/articles/JUnit/article.html) for testing. Pull Requests(PR) should attempt to integrate within that framework as appropriate.
OilMod is a large project and what seems simple to a PR author at the time of writing may easily be overlooked by other authors and updates. Including unit tests with your PR
will help to ensure the PR can be easily maintained over time and encourage the Spigot team to pull the PR.

* There needs to be a new line at the end of every file.
* Imports should be organised in a logical manner.
    * Do not group packages unless already grouped.
    * All new imports should be within existing OilSpigot comments if any are present. If not, make them.
    * __Absolutely no wildcard imports.__
    * If you only use an import once, don't import it. Use the fully qualified name.

For example:
```java
    import java.io.ByteArrayInputStream;
    import java.io.DataInputStream;
    import java.io.IOException;
    import java.io.UnsupportedEncodingException;
    import java.util.concurrent.ExecutionException;
    import java.util.concurrent.atomic.AtomicIntegerFieldUpdater;

    import org.bukkit.Bukkit;
    import org.bukkit.Location;
    import org.bukkit.craftbukkit.CraftWorld;
    import org.bukkit.craftbukkit.inventory.CraftInventoryView;
    import org.bukkit.craftbukkit.inventory.CraftItemStack;
    import org.bukkit.craftbukkit.util.LazyPlayerSet;
```

Any questions about these requirements can be asked in #spigot-dev in IRC.

Making Changes to Minecraft
---------------------------
Importing new NMS classes to OilSpigot is actually very simple.

1. Find the `work/decompile-XXXXXX` folder in your BuildTools folder.
2. Find the class you want to add in the `net/minecraft/server` folder and copy it.
3. Copy the selected file to the `src/main/java/net/minecraft/server` folder in OilSpigot.
4. Implement changes.
5. Run `makePatches.sh` to create a new patch for the new class.
    * _Be sure that Git recognizes the new file. This might require manually adding the file._
6. Commit new patch.

Done! You have added a new patch for OilSpigot!

**Making Changes to NMS Classes**

Bukkit/CB employs a Minimal Diff policy to help guide when changes should be changed to Minecraft and what those changes should be.
This is to ensure that any changes have the smallest impact possible on the update process whenever a new Minecraft version is released.
As well as the Minimal Diff Policy, *every* change made to a Minecraft class must be marked with the appropriate OilSpigot comment.
At no point should you rename an existing/obfuscated field or method. All references to existing/obfusacted fields/methods should be marked with the `// PAIL rename` comment.
Mapping of new fields/methods are done when there are enough changes to warrant the work. (Any questions can be asked in #spigot-dev in [IRC](https://www.spigotmc.org/wiki/irc-guide/))

__*Key Points*__:
* All additions to patches must be accompanied by an appropriate comment.
* To avoid large patches, avoid adding methods where possible. We prefer making fields public over adding methods in patches.
  * If you *have* to add a method to a patch, please explain why in the Pull Request description.
* __Never__ rename an existing field or method. If you want something renamed, include a ```PAIL rename``` comment
* Converting a method/class from one access level to another (i.e. private to public) is fine as long as that method is not overridden in subclasses.
  * If a method is overridden in its' subclasses, create a new method that calls that method instead (along with appropriate OilSpigot comments).

**Minimal Diff Policy**

The Minimal Diff Policy is key to any changes made within Minecraft classes. When people think of the phrase "minimal diffs", they often take it
to the extreme - they go completely out of their way to abstract the changes they are trying to make away from editing Minecraft's classes as much as possible.
However, this is not what is meant by "minimal diffs". Instead, when trying to understand this policy, it helps to keep in mind its goal: to reduce the impact of changes we make
to Minecraft's internals have on our update process.

To put it simply, the Minimal Diffs Policy simply means to make the smallest change in a Minecraft class possible without duplicating logic.

Here are a few tips you should keep in mind, or common areas you should focus on:

* Try to avoid duplicating logic or code when making changes.
* Try to keep your changes easily discernible - don't nest or group several unrelated changes together.
    * All changes must be surrounded by [OilSpigot comments](#oilspigot-comments).
* If you only use an import once within a class, don't import it and use a fully qualified name instead.
* Try to employ "short-circuiting" of logic if at all possible. This means you should force a conditional to be the value needed to side step the code block to achieve your desired effect.

__For example, to short circuit this:__
```java
if (!this.world.isClientSide && !this.isDead && (d0*d0) + d1 + (d2*d2) > 0.0D) {
    this.die();
    this.h();
}
```
__You would do this:__
```java
if (false && !this.world.isClientSide && !this.isDead && (d0*d0) + d1 + (d2*d2) > 0.0D) {
    this.die();
    this.h();
}
```

* When adding a validation check, this applies everywhere not just in Minecraft classes, see if the Validate package has a better, more concise method you can use instead.
    * The Preconditions package works just as well. Either are acceptable; though these are, by no means, the only accepted validation strategies.

__For example, you should use:__
```java
Validate.notNull(sender, "Sender cannot be null");
```
__Instead of:__
```java
if (sender == null) {
    throw new IllegalArgumentException("Sender cannot be null");
}
```

* When the change you are trying to make involves removing code, or delegating it somewhere else, instead of removing it, you should comment it out.

__For example:__
```java
/*OilSpigot start - special case dropping so we can get info from the tile entity*/
public void dropNaturally(World world, int i, int j, int k, int l, float f, int i1) {
    if (world.random.nextFloat() < f) {
        ItemStack itemstack = new ItemStack(Item.SKULL, 1, this.getDropData(world, i, j, k));
        TileEntitySkull tileentityskull = (TileEntitySkull) world.getTileEntity(i, j, k);

        if (tileentityskull.getSkullType() == 3 && tileentityskull.getExtraType() != null && tileentityskull.getExtraType().length() > 0) {
            itemstack.setTag(new NBTTagCompound());
            itemstack.getTag().setString("SkullOwner", tileentityskull.getExtraType());
        }

        this.b(world, i, j, k, itemstack);
    }
}
/*OilSpigot end*/

public void remove(World world, int i, int j, int k, int l, int i1) {
    if (!world.isStatic) {
        /* OilSpigot start - drop item in code above, not here*/
        if ((i1 & 8) == 0) {
            ItemStack itemstack = new ItemStack(Item.SKULL, 1, this.getDropData(world, i, j, k));
            TileEntitySkull tileentityskull = (TileEntitySkull) world.getTileEntity(i, j, k);

            if (tileentityskull.getSkullType() == 3 && tileentityskull.getExtraType() != null && tileentityskull.getExtraType().length() > 0) {
                itemstack.setTag(new NBTTagCompound());
                itemstack.getTag().setString("SkullOwner", tileentityskull.getExtraType());
            }

            this.b(world, i, j, k, itemstack);
        }
        /*OilSpigot end */

        super.remove(world, i, j, k, l, i1);
    }
}
```

#### OilSpigot Comments

Changes to a Minecraft or CraftBukkit class should be clearly marked using OilSpigot comments.

* All OilSpigot comments should be capitalised appropriately. (i.e. OilSpigot, not OilMod/oilspigot, etc.)
* If the change only affects one line of code, use an end of line OilSpigot comment

__Examples:__

If the change is obvious, then you need a simple end of line comment.
```java
if (true || minecraftserver.getAllowNether()) { // OilSpigot
```

Every reference to an obfuscated field/method in NMS should be marked with:
```java
// PAIL rename newName
```
All PAIL rename comments must include a new name.

If, however, the change is something important to note or difficult to discern, you should include a reason at the end of the comment
```java
public int fireTicks; // PAIL private -> public
```
Changing access levels must include a PAIL comment indicating the previous access level and the new access level.

If adding the OilSpigot comment negatively affects the readability of the code, then you should place the comment on a new line *above* the change you made.
```java
// OilSpigot
if (!isEffect && !world.isStatic && world.difficulty >= 2 && world.areChunksLoaded(MathHelper.floor(d0), MathHelper.floor(d1), MathHelper.floor(d2), 10)) {
```

* If the change affects more than one line, you should use a multi-line OilSpigot comment.

__Example:__

The majority of the time, multi-line changes should be accompanied by a reason since they're usually much more complicated than a single line change.
*If the change is something important to note or difficult to discern, you should include a reason at the end of line OilSpigot comment.*
```java
/*OilSpigot start - special case dropping so we can get info from the tile entity*/
public void dropNaturally(World world, int i, int j, int k, int l, float f, int i1) {
    if (world.random.nextFloat() < f) {
        ItemStack itemstack = new ItemStack(Item.SKULL, 1, this.getDropData(world, i, j, k));
        TileEntitySkull tileentityskull = (TileEntitySkull) world.getTileEntity(i, j, k);

        if (tileentityskull.getSkullType() == 3 && tileentityskull.getExtraType() != null && tileentityskull.getExtraType().length() > 0) {
            itemstack.setTag(new NBTTagCompound());
            itemstack.getTag().setString("SkullOwner", tileentityskull.getExtraType());
        }

        this.b(world, i, j, k, itemstack);
    }
}
/*OilSpigot end*/
```
Otherwise, if the change is obvious, such as firing an event, then you can simply use a multi-line comment.
```java
    /*OilSpigot start*/
    BlockIgniteEvent event = new BlockIgniteEvent(this.cworld.getBlockAt(i, j, k), BlockIgniteEvent.IgniteCause.LIGHTNING, null);
    world.getServer().getPluginManager().callEvent(event);

    if (!event.isCancelled()) {
        world.setTypeIdUpdate(i, j, k, Block.FIRE);
    }
    /*OilSpigot end*/
```
* All OilSpigot comments should be on the same indentation level the code block it is in.

__Imports in Minecraft Classes__
* Do not remove unused imports if they are not marked by OilSpigot comments.


Creating a Pull Request
-----------------------

##### Pull Request Title Example

The first line in a Pull Request(PR) message is an imperative statement briefly explaining what the PR is achieving.
If the PR fixes a bug, or implements a new feature as requested from the [JIRA](http://hub.spigotmc.org/jira/), then it should reference that ticket.
This is accomplished by simply type SPIGOT-####, where #### is the ticket number. (i.e. SPIGOT-3510).
You can reference multiple tickets in a single commit message; for example: "SPIGOT-1, SPIGOT-2" without closing punctuation.

__For Example:__
* SPIGOT-3510: Velocity broken for certain entities
* MC-111753, SPIGOT-2971: Brewing stand not reloading

As you can see, Minecraft tickets can be referenced by including the appropriate ticket number (i.e. MC-111753)

##### Pull Request Message Expectations

The body of a PR needs to describe how the ticket was resolved, or if there was no ticket, describe the problem itself.
If a PR is for both Bukkit and CraftBukkit it should include a link to the appropriate PR. [Read this to learn how to link to a PR.](https://confluence.atlassian.com/bitbucketserver053/markdown-syntax-guide-938022413.html?utm_campaign=in-app-help&utm_medium=in-app-help&utm_source=stash#Markdownsyntaxguide-Linkingtopullrequests)


##### Submitting the Changes

* Push your changes to a topic branch in your fork of the repository.
* Submit a pull request to the relevant repository in the Spigot organization.
    * Make sure your pull request meets our [code requirements.](#code-requirements)
* If you are fixing a JIRA ticket, be sure to update the ticket with a link to the pull request.
* Ensure all changes (including in patches) have our Minimal Diff Policy in mind.


##### Tips to Get Your Pull Request Accepted

Making sure to follow the conventions!

* Your change should fit with Bukkit's goals.
* Make sure you follow our conventions to the letter.
* Check for formatting errors. They may be invisible, but [we notice.](https://hub.spigotmc.org/stash/projects/SPIGOT/repos/craftbukkit/pull-requests/298/diff)
* Provide proper JavaDocs where appropriate.
    * JavaDocs should detail every limitation, caveat, and gotcha the code has.
* Provide proper accompanying documentation where appropriate.
* Test your code and provide material.
    * For example: adding an event? Test it with a plugin and provide us with the source.
* Make sure you follow our conventions to the letter.

__Note:__ The project is on a code freeze leading up to the release of a Minecraft update in order to give the team a static code base to work with.

Useful Resources
----------------

* [An example pull request demonstrating the things we look out for](https://hub.spigotmc.org/stash/projects/SPIGOT/repos/craftbukkit/pull-requests/365/overview)
* [JIRA, our bug tracker](http://hub.spigotmc.org/jira/)
* [Join us on IRC - #spigot-dev @ irc.spi.gt](https://www.spigotmc.org/wiki/irc-guide/)
