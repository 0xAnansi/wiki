# Introduction

The following tutorial aims at making you able to delete your reddit history, partially or entirely, while bypassing the reddit's API limitation making you able to remove only the last 1000 messages that you made.

## Why?

Reddit's business is based on using the data that you freely submit to it, to resell them to data aggregators and advertisers.
It is merely good digital hygiene to remove data that you spread to the internet as much as possible, but it is also good, even better, as a protest mean against the API changes announced by Reddit, than a subreddit blackout.

While a subreddit blackout can negatively impact Reddit's image, it's only temporary, and the mods that trigger the blackouts can be removed and replaced on a whim.

This is not the case for your data, and the data that makes these subreddits.

## Will this kill my karma?
No. The karma stays.

## Is it irreversible?

Yes and no. 

This will remove your comments, and removed comments cannot be restored (afaik).

You will still be in possession of your comment archive though, which makes you able to repost every single comment through automated means in each threads it was deleted from, but you might be limited by thread archiving, and available tools to do so.

So TL;DR, your data won't be lost, but won't be easily publishable.

# Requesting your data from reddit

The first step in removing your data, is actually getting it all from reddit.
You can achieve this by making a request at the following page, while logged-in as the user you want the data of: [Request your reddit data](https://www.reddit.com/settings/data-request)

You need to pick "General Data Protection Regulation (GDPR)" in the type of request to get the most exhaustive one.

For the range of data, select "I want data from my full time at Reddit".

This is done asynchronously and you will need to check your reddit messages until you receive the download link.
Note that it can only done once per 30 days.

# Using your data dump to remove your history

Once you got your data archive from reddit, you'll be in the possession of the full list of messages that you ever wrote, far beyond the 1000 entries that you could have fetched through the API.

From there, you can either build a tool that will delete the comment from the API based on the ID of each of the comments in the dump, or use a tool that has already made it easy.

For my own use, I use the tool called redact, [available on their website](https://redact.dev/)
Disclaimer: I am not affiliated with redact nor do I receive anything from them for advertising their tool.

Note that Redact is free, but is not open-source, so use at your own risks. 

Why Redact? Because it has the useful feature of being able to take the archive sent by reddit directly as input to clear your reddit account. It's also available on all the existing OS that I can think of, so no matter if you want to do it by phone, from windows, mac or linux, a client is available.

## Configuring the reddit removal service
Pick the reddit service and click on "Add account". This will bring the login page where you need to log-in to the account you want to delete data from.

![[Screenshot_787.png]]

Once logged-in, you will need to authorize Redact to access your data through your account:

![[Pasted image 20230614165145.png]]

And that's it!

# Starting the removal of your data

You have a lot of option concerning the time period you want your data deleted.
![[Pasted image 20230614165306.png]]

For this use, I would advise using "Relative Date", from "All time" to "3 months ago".
This will remove your historical data while keeping your most recent activity you're most likely to be replied to.

In the "Comments" section, you will notice this option:
![[Pasted image 20230614165527.png]]

This is where you put the archive that you got from Reddit.
This will make Redact non-reliant on the Reddit's API to fetch comments, and bypass the 1000 comments limit that the API returns.

You have then a plethora of options to better configure what you want to delete, but here, we're mostly interested in wiping everything.

![[Pasted image 20230614165708.png]]

In the section "Action to take", pick "Edit comments". This will edit with garbage texte, then remove the comment, which is supposed to make the original comment unrecoverable (this might not be true, but this is why it's there so better try)

![[Pasted image 20230614165845.png]]

In "Custom Edit Text" you can enter what you want your comment to be replaced with if random text is not to your liking.
You can use any of the community approved ones such as, but not limited to:
- Fuck Spez
- Due to the changes in the API limitations and access, this comment has been removed from reddit
- Removed for privacy
- Deez nutz
- Etc.

![[Pasted image 20230614170055.png]]

You can also specify karma upper and lower limit to delete, but for this, we'll delete everything.

![[Pasted image 20230614170125.png]]

You can also select your comments through wordlist, but once again, this is not our case.
![[Pasted image 20230614170202.png]]

We are now done with the configuration.

We can either check if this is right by running a "Preview Mode" run, or yoloing and directly delete everything.

![[Pasted image 20230614170246.png]]
Then, start the process.
![[Pasted image 20230614170303.png]]

And accept that Redact will not be responsible if you deleted the secret nuclear launch codes that you transferred from your toilets to a comment on Reddit.

![[Pasted image 20230614170429.png]]

This will now run for as long as needed to remove everything.

# TL;DR

- Get your data archive from https://www.reddit.com/settings/data-request
- Put it in https://redact.dev/
- Configure and launch deletion
- ???
- ~~Fuck Spez~~ Profit