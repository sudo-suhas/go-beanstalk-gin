# go-beanstalk-gin

> A demo webapp using Go, gin and Elastic Beanstalk.

This project demonstrates the deployment of a simple webapp built using
[`gin`][gin] to [Elastic Beanstalk][elastic-beanstalk]. Dependencies are
managed using [`dep`][dep].

This tutorial should be usable on Windows and *nix. Most of the tools, like the
[Go tool][go-tool] and [EB CLI][eb-cli] are cross platform compatible. For some
of the commands which aren't available on Windows, alternate commands have been
included. I successfully tested it on Windows 10 and Ubuntu.

## Docs

The steps have been divided and documented in the following stages:

1. [Create gin webapp](docs/create-gin-webapp.md)
1. [Deploy to Elastic Beanstalk](docs/deploy-to-elastic-beanstalk.md)

## License

MIT © [Suhas Karanth][sudo-suhas]

[gin]: https://github.com/gin-gonic/gin
[elastic-beanstalk]: https://aws.amazon.com/documentation/elastic-beanstalk/
[dep]: https://github.com/golang/dep
[go-tool]: https://golang.org/cmd/go/
[eb-cli]: http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3.html
[sudo-suhas]: https://github.com/sudo-suhas
