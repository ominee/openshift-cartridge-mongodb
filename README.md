# Custom MongoDB cartridge for OpenShift

This is a custom OpenShift cartridge providing MongoDB > 3.0.3, forked from [icflorescu/openshift-cartridge-mongodb](http://github.com/icflorescu/openshift-cartridge-mongodb).

## Why

Because the standard OpenShift MongoDB cartridge is stuck at 2.4.

## When to use

When you need a quick and unsofisticated solution to run your application with the latest MongoDB version.

## How to

To install this cartridge in your existing OpenShift application, go to **"See the list of cartridges you can add"**, paste the URL below in **"Install your own cartridge"** textbox at the bottom of the page and click "Next".

    http://cartreflect-claytondev.rhcloud.com/github/ominee/openshift-cartridge-mongodb

Then you can use `MONGODB_URL` environment variable to connect from an application running in the main web cartridge.

To keep things flexible, the connection string stored in `MONGODB_URL` ends with `/` and **does not include a database name**. Use your application logic to name/create your database(s) as needed.

For instance, here's how you'd do it in a Node.js application using [Mongoose ODM](http://mongoosejs.com/):

    var mongoose = require('mongoose');
    ...
    // Mongoose will create database_name if necessary
    mongoose.connect(process.env.MONGODB_URL + 'database_name', { db: { nativeParser: true } });

## Notes

- Can't guarantee this cartridge is production-ready. Some people use it though (on **their own responsibility**).
- This is a lean cartridge. To save space, just `mongod` is installed. No client libraries, no `mongo` console. If you need external access to your data, use `rhc port-forward`.
- By default, the underlying MongoDB instance will accept unauthenticated access, which should be fine for most typical usage scenarios. See [the discussion here](https://github.com/ominee/openshift-cartridge-mongodb/issues/1) for more info.
- Can't think of a way to make this cartridge auto-updatable. There's nothing like [semver.io](http://semver.io) for MongoDB. For now we'll just have to use `mongodump`, destroy the cartridge, install the new version, then do a `mongorestore`.
- Don't hesitate to make a pull-request with an updated version in [this file](https://github.com/ominee/openshift-cartridge-mongodb/blob/master/metadata/manifest.yml#L4) if you notice this cartridge version is behind  the latest stable [official MongoDB linux binary](http://www.mongodb.org/downloads).

## FAQ

**Q**: I'm getting the error *Cannot download, must be smaller than 20480 bytes* while trying to deploy the cartridge to OpenShift. What am I doing wrong?

**A**: You're probably trying to use the URL `https://github.com/ominee/openshift-cartridge-mongodb` instead of
`http://cartreflect-claytondev.rhcloud.com/github/ominee/openshift-cartridge-mongodb`. A common mistake for people not paying sufficient attention while trying to use a custom cartridge for the first time.

## Related

Since you're here, chances are you might also be interested in this [custom Node.js cartridge](https://github.com/ominee/openshift-cartridge-nodejs).

## License

The [MIT License](http://github.com/ominee/openshift-cartridge-mongodb/LICENSE).
