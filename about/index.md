---
layout: page
title: About H2O
navigation: true
logo: 'assets/images/logo-dark.png'
current: about
---
H2O builds on top of two core frameworks: [OrbitDB](https://github.com/orbitdb/orbit-db/) and [Ocean Protocol](https://github.com/oceanprotocol/).

OrbitDB is a peer-to-peer distributed database for [IPFS](https://ipfs.io/) which serves as H2O's core data store. Ocean Protocol is a decentralized data exchange protocol to democratise and unlock data for AI.

#### Dataflow

The best way to understand how the components of H2O fit together is by following the flow of data.

![Dataflow](/assets/images/dataflow.png)

Let’s say we have some data. First, we need to make it a part of the decentralised web. To get the dataset on IPFS, we use [H2O-Host](https://www.github.com/OutlierVentures/H2O-Host/). A lightweight application, H2O-Host creates an OrbitDB database from JSON-formatted data and sets us up as a peer (or node) on IPFS. Our database is referenced by an IPFS multihash, and anyone with this address can get a copy through OrbitDB.

The H2O-Host component includes a data generation script for testing, as well as an example dataset of uber pickup locations.

Once a few people have a copy of our database, we get to see one of the standout features of IPFS and OrbitDB. When someone goes to our multihash, the closest peer to them is intelligently selected to serve them the data. This means as the network grows, the speed of serving content collectively improves. Better still, the data isn’t really stored anywhere in particular, but bounces around as it is needed. This massively improves censorship resistance and the ability to withstand denial of service attacks.

Ok, we’ve made the dataset available. Where does H2O come in?

We want a nice clustered dataset for AI that’s available to everyone. H2O is going to process it for us, but first it needs a copy from IPFS. Fortunately, OrbitDB makes this easy for us: we enter the address of our desired database in the H2O UI and the backend fetches the database by calling Orbit’s replicate function.

Let’s say our crypto-economics team in Toronto wants to send some unstructured, clusterable data for our London tech team to process for AI. With H2O-Host running in Toronto, replicating the data to H2O in London will take us around 10 seconds. This is unprecedented: IPFS is already demonstrating its potential as a viable alternative to HTTP, a protocol that’s been in development since 1989.

Now we have a copy of our dataset in H2O. Let’s cluster it.


#### Machine Learning in H2O

At this point, we probably want to take a look at our data. Fortunately, H2O visualises it for us by plotting it as soon as it has a copy. We can take a look and tell the algorithm how many clusters it should aim for.

The clustering technique H2O implements is K-means. K-means does what it says on the tin: it finds k means, or cluster centres. To do this, the algorithm divides a dataset into groups having approximately the same number of points closest to them. In other words, K-means looks for data density. This approach is known as vector quantisation, and allows K-means to assign each datapoint to a cluster centre, creating a grouping.

![Clustering](/assets/images/clustering.gif)
<center>K-means visualization by Andrey A. Shabalin</center>
<p></p>

K-means has a high computational complexity, which means computing it is difficult. H2O computes the problem in Python using the open-source machine learning library SciKit Learn. SciKit Learn makes a vast array of machine learning algorithms available at our fingertips and is currently one of the most powerful tools for developing AI.

While Python drives the back-end of H2O (along with some NodeJS), the front end is written in Angular and interfaces with the back-end using Flask. This allows H2O to quickly relay and visualise relevant information, such as rendering the clustered output following a K-means computation.

To relate this to real data, we can use the Uber pickup location data for New York included in the H2O-Host repo. et's say we want to start decentralised ride sharing app. If we own 5 cars, we take a sample of the pickup data, load it in H2O and specify 5 as the number of clusters. Plotting the resulting dataset on a map of the city, we can see the car deployment locations which minimise customer wait time and fuel costs.

![Map](/assets/images/map.png)

The processed data, now monetizable on Ocean Protocol’s global data market, is ready to be published.

#### Serving the data

H2O makes use of Ocean Protocol’s Squid API for Python to register datasets on the blockchain. Squid is a library for writing applications that speak to and interact with Ocean Protocol, coming in JavaScript and Python variants. H2O uses the latter for quick interfacing with its machine learning components, which already run in Python. Asset price, name, description and author can all be set directly by the user in the app.

H2O integrates with the Kovan testnet, and developers who opt to use it are fed an Etherscan link to their published smart contracts when registering assets.

Note that Ocean Protocol, as a global data marketplace, does not host published datasets. The network demands that users host their own data and currently only supports Azure storage, though several alternatives, including decentralised options, are on the roadmap. H2O interfaces directly with Azure storage using the Azure Python SDK. In H2O, we’re able to directly upload our dataset in a minimised JSON format using only an account name and access key. The dataset is written straight to an Azure blob and a machine-readable download link is passed to the Ocean blockchain. This link is also fed back to us, the user, so we can download our clustered data immediately should we want to. 

In addition to Azure hosting, H2O includes Proof-of-Concept OrbitDB hosting. After all, OrbitDB can serve a user data in very much the same way that Azure can – it just isn’t supported by Ocean Protocol yet. Guides for getting started, whether with Azure or OrbitDB, can be found in the docs. Even if you don’t have access to any data yourself, H2O-Host comes with scripts for generating clusterable data that you can feed into H2O, so test away - check out the [GitHub](https://www.github.com/OutlierVentures/H2O) and [live version](https://live.h2o.outlierventures.io).
