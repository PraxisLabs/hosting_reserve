# Site Reserve

This module is an extension of hosting\_saas that creates a stockpile of sites to accelerate the delivery of new sites.

The point of this module is to enhance the performance of an Aegir-based SaaS service. You can use this module's make\_site function instead of using the basic hosting\_saas function.

This module was originally developped by [Praxis Labs](http://praxis.coop) for the [GetOpenOutreach](http://getopenoutreach.com) service.

## What is this witchcraft?

In short, this is how this module works:

1. Make clones of the template site.
2. When someone asks for a new site, take one of these clones and give it an alias. Hurray, the person can use their new site!
3. [When no one is looking] Migrate the site to its permanent URL.
4. Delete old clones.
5. Repeat

## But why?

Cloning complex sites can take anywhere from a 1-5 minutes. Running a simple Verify barely ever takes more than 30 seconds. Gotta go fast.

## Installation

All you need is this module and its prerequisites:

* [Aegir](http://aegirproject.org) (obviously)
* [hosting\_saas](https://drupal.org/project/hosting_saas) : Provides general settings for a SaaS service.

You'll probably need your own module or glue code to receive the requests for new sites. Maybe check out [hosting\_restapi](https://github.com/coopsymbiotic/hosting_restapi).

## Configuration details

You'll see the settings for this module in Hosting -> Site reserve.

### Reserve

When we talk about the "reserve", we're talking about unused clones.

### Full reserve size

The maximum number of sites in the reserve.

### Minimum reserve size

When there are fewer than this many clones in the reserve, we ignore the refilling time window restrictions. This means that we'll make new clones as soon as possible. You can see this as an "emergency reserve".

If you set this to the same number as the full reserve size, the cloning window times will be inconsequential (but still respected for the deletion of old clones). It doesn't make sense to set this higher than the Full reserve size.

The default setting is 1 because only the busiest services would need more than that in reserve at any given time.

### Clone life time

Clones will be deleted after they've been in the "reserve" this long.

We usually need to delete old clones because we will reguraly make changes to the template site, so clones will be out of date.

If you set this to infinite (0), you'll need to manually deleted the existing clones when you make changes.

### Cloning window

In the settings you'll see "Time window for refilling".

The point of this module is to have pre-made clones that we can quickly alias to serve sites very fast.

But when do we make the clones? If we make them during the busy hours, we run a higher risk of a clone operation running when we receive the new site request. Since the tasks are run sequentially, we'd have to wait for the clone operation to finish, making this no more efficient than the normal clone method.

This is why the default setting is in the middle of the night since that's when there's the least traffic. (Note that your server time may not be the same as your local time.)

This is also when the old clones are deleted. The default site expiration time is 23h because cloning generally takes a few minutes, so if we set it to 24h we could gradually slip until at some point there's no new clone for 48h.

## FAQ

Q: **How does this module know which site to clone?**

A: We just use the hosting\_saas settings. See Hosting -> SaaS.

Q: **Can I just use this module without configuring it?**

A: Pretty much. It will use the hosting\_saas settings, so you'll still need to configure that first.

Q: **What happens when I request a new site and there are no clones?**

A: The module will use the default site creation function from the hosting\_saas module.

Q: **Why don't we just use an aliased clone permanently? Why do we need to migrate?**

A: Historically, some errors have appeared only when using the site through its alias. No alias = fewer moving parts. Also, managing a thousand sites named hosting-reserve-N is a way bigger pain than if they have the proper title.

Q: **I think this should do [X].**

A: Pull requests welcome. If you're not a coder, submit an issue and we'll see what we can do. If you need this for commercial use, consider [sponsoring the development](http://praxis.coop/en/contact).
