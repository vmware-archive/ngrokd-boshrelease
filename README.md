# ngrokd-boshrelease is no longer actively maintained by VMware.

# BOSH Release for ngrok daemon

One of the fastest ways to get a [ngrok](https://github.com/inconshreveable/ngrok) server running on any cloud infrastructure is too deploy this BOSH release.

## Usage

### Upload the BOSH release

To use this bosh release, first upload it to your bosh:

```
bosh target BOSH_HOST
git clone https://github.com/cf-platform-eng/ngrokd-boshrelease.git
cd ngrokd-boshrelease
bosh upload release releases/ngrokd/ngrokd-1.yml
```

### Create a BOSH deployment manifest

Now create a deployment file using the files at the [examples](https://github.com/cf-platform-eng/ngrokd-boshrelease/tree/master/examples) directory as a starting point.

`ngrok` provides secure tunnels via TLS, so **you'll need an SSL certificate matching your server domain** in order to deploy this BOSH release
(see "[How to run your own ngrokd server](https://github.com/inconshreveable/ngrok/blob/master/docs/SELFHOSTING.md)").
The SSL certificate (key and certificate) must be provided at the deployment manifest

If you want to use a self-signed cert then you'll need to recompile the ngrok client with your signing CA (see
"[ngrokd with a self-signed SSL certificate](https://gist.github.com/lyoshenka/002b7fbd801d0fd21f2f)").

### Deploy using the BOSH deployment manifest

Using the previous created deployment manifest, now we can deploy it:

```
bosh deployment path/to/deployment.yml
bosh -n deploy
```

## Contributing

In the spirit of [free software](http://www.fsf.org/licensing/essays/free-sw.html), **everyone** is encouraged to help improve this project.

Here are some ways *you* can contribute:

* by using alpha, beta, and prerelease versions
* by reporting bugs
* by suggesting new features
* by writing or editing documentation
* by writing specifications
* by writing code (**no patch is too small**: fix typos, add comments, clean up inconsistent whitespace)
* by refactoring code
* by closing [issues](https://github.com/cf-platform-eng/ngrokd-boshrelease/issues)
* by reviewing patches


### Submitting an Issue
We use the [GitHub issue tracker](https://github.com/cf-platform-eng/ngrokd-boshrelease/issues) to track bugs and features.
Before submitting a bug report or feature request, check to make sure it hasn't already been submitted. You can indicate
support for an existing issue by voting it up. When submitting a bug report, please include a
[Gist](http://gist.github.com/) that includes a stack trace and any details that may be necessary to reproduce the bug,
including your gem version, Ruby version, and operating system. Ideally, a bug report should include a pull request with
 failing specs.


### Submitting a Pull Request

1. Fork the project.
2. Create a topic branch.
3. Implement your feature or bug fix.
4. Commit and push your changes.
5. Submit a pull request.

### Create new release

#### Creating a final release

If you need to create a new final release, you will need to get read/write API credentials to the [@cloudfoundry-community](https://github.com/cloudfoundry-community) s3 account.

Please email [Dr Nic Williams](mailto:&#x64;&#x72;&#x6E;&#x69;&#x63;&#x77;&#x69;&#x6C;&#x6C;&#x69;&#x61;&#x6D;&#x73;&#x40;&#x67;&#x6D;&#x61;&#x69;&#x6C;&#x2E;&#x63;&#x6F;&#x6D;) and he will create unique API credentials for you.

Create a `config/private.yml` file with the following contents:

``` yaml
---
blobstore:
  s3:
    access_key_id:     ACCESS
    secret_access_key: PRIVATE
```

You can now create final releases for everyone to enjoy!

```
bosh create release
# test this dev release
git commit -m "updated ngrokd"
bosh create release --final
git commit -m "creating vXYZ release"
git tag vXYZ
git push origin master --tags
```

## Copyright

See [LICENSE](https://github.com/cf-platform-eng/ngrokd-boshrelease/blob/master/LICENSE) for details.
Copyright (c) 2014 [Pivotal Software, Inc](http://www.pivotal.io/).
