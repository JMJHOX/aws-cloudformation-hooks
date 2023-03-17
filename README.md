# aws-cloudformation-hooks
This tutorial is about how to develop your hooks without losing your humanity, step-by-step

This tutorial will be  divided in three sections. I hope it works for you.



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-tutorial">About The Tutorial</a>
    </li>
    <li>
      <a href="#getting-started">Before Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
        <li>
      <a href="#getting-started">Working on it!</a>
      <ul>
        <li><a href="#construct">Constructing our hooks</a></li>
        <li><a href="#building">Building & Verifying</a></li>
        <li><a href="#testing">Deployment & Testing </a></li>
      </ul>
    </li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Tutorial

Nothing to say.

<!-- GETTING STARTED -->
## Getting Started

### Prerequisites
For this tutorial, you will require some knowledge on this topics:


* Docker
* Python(Or your most favorite language)
* Lambda
* Cloudformation(Basics)
* AWS (Basics)


### Installation

To start with the installation, with your start with building our docker enviroment to start to develop our hooks.
For this task, we can start an EC2 Instance for this task, but there is an alternative that is mount an Docker image with EC2 Image on it.

We´re going to create a Dockerfile in our root project in our machine and copy/paste this snippet:

```
FROM amazonlinux:2

RUN yum -y update \
    # systemd is not a hard requirement for Amazon ECS Anywhere, but the installation script currently only supports systemd to run.
    # Amazon ECS Anywhere can be used without systemd, if you set up your nodes and register them into your ECS cluster **without** the installation script.
    && yum -y install systemd \
    && yum clean all

RUN cd /lib/systemd/system/sysinit.target.wants/; \
    for i in *; do [ $i = systemd-tmpfiles-setup.service ] || rm -f $i; done

RUN rm -f /lib/systemd/system/multi-user.target.wants/* \
    /etc/systemd/system/*.wants/* \
    /lib/systemd/system/local-fs.target.wants/* \
    /lib/systemd/system/sockets.target.wants/*udev* \
    /lib/systemd/system/sockets.target.wants/*initctl* \
    /lib/systemd/system/basic.target.wants/* \
    /lib/systemd/system/anaconda.target.wants/*

RUN amazon-linux-extras install epel docker && \
    systemctl enable docker

CMD ["/usr/sbin/init"]
```
Why we should have to do this? well, we have a great reason for this.
Turns out, Cloudformation works with AWS Lambda "under the hood" to execute our hooks when an Stack is created/deleted/updated.
That´s why we can´t use any environment to develop our hooks, It needs to be worked like we are creating an AWS Lambda Layer.

Now, we have to build our image in our machine and run it! And remember we need to get into our docker environment to start to work.

```
docker build -t {my_image_name} .
docker run -d {my_image_name}
```

Finally, we need to access it:
```
docker exec -it {my_image_name} bash
```

Next, we have to create our python virtual environment inside our docker image. For this:

We install the necessary libraries:
```
pip install virtualenv
```
Use the following command in our root to create the virtual environment
```
virtualenv env
```

And activate it
```
source env/bin/activate
```
and We´re almost done!
Remember, our principal objective is to develop our own hooks. We must have to learn the basics first.
So we´ll be using an base hook resource from the [aws-cloudformation-samples](https://github.com/aws-cloudformation/aws-cloudformation-samples/tree/main/hooks/python-hooks/resource-tags). 

We´ll need to download this sample into our machine, we can download the .zip file and move it to our machine with our mouse or use the CLI:
```
wget http://github.com/[username]/[repo]/archive/master.zip
unzip file_name
```
This command downloads the repository as an .zip file

PENDING TUTORIAL


<p align="right">(<a href="#top">back to top</a>)</p>



<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".
Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- LICENSE -->
## License

aws-cloudformation-hooks is distributed under the [MIT License](https://opensource.org/licenses/MIT).

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- CONTACT -->
## Contact

JMJHOX  - josemiguelaparicio507@gmail.com

Tutorial Link: [https://github.com/JMJHOX/aws-cloudformation-hooks](https://github.com/JMJHOX/aws-cloudformation-hooks)

<p align="right">(<a href="#top">back to top</a>)</p>

<!--





## Code of Conduct

In order to ensure that the Date-simplify community is welcoming to all, please review and abide by the [Code of Conduct](https://github.com/JMJHOX/date-simplify/blob/development/docs/CODE_OF_CONDUCT.md).


