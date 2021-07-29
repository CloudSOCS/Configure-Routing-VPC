Configure Routing VPC
---

In this article, we're going to configure routing in our custom VPC.
We have three public subnets and three private subnets.And each of them has an address block assigned.

![block](/pic/1.png)

We're going to attach an Internet gateway and then specify the Internet gateway in the route table. And that route table will apply for our public subnets. We have a separate route table that we're going to create. And that's going to be for our private subnets.

So we need to attach the route table to the relevant subnets.

![route](/pic/2.png)


The private route table will only have this local route.

![route local](/pic/4.png)


![route](/pic/3.png)

Any EC2 instance deployed into these private subnets will be able to route to any other IP address within this range, whether it's in a public subnet or a private subnet. Routing happens using that implicit router between any of these subnets, but it doesn't have any route to an Internet gateway, which means it can't access the outside world. So there's no chance that instances within these subnets can actually access the Internet directly.
In another tutorial I will show you how to fix this using a NAT gateway.


Now back in the AWS management console. I'm going to Internet gateways. There's already an Internet gateway and that one is attached to our default VPC.

![gate](/pic/5.png)

Now create a new Internet gateway. Call it my-vpc-igw and then we're going to choose create Internet gateway.

![igw](/pic/6.png)


We've got the Internet gateway created, but we now need to attach it and actually tells us at the top. So just click on this button in the top right-hand corner, attach to a VPC.

![attach](/pic/7.png)

When we click in this box here, we should see the VPC ID pop up.

![box](/pic/8.png)

Click on attach Internet gateway and it's now attached to the VPC. It's not useable until you actually add an entry into a route table.

Go to our route table,

![choose](/pic/9.png)

choose the route table for our custom VPC. I can see that it's this one here. If I click on routes, we can see we have that default route. That's the local one, which means that the implicit router will route traffic to any IP address in the CIDR block for the VPC.

![newrout](/pic/10.png)

I'm going to click on edit routes


Then on add routes, and then what we're going to do is specify 0.0.0.0/0. that's the any IP address. So any IP address outside of this range is going to go to the target we specify here. If you choose the dropdown, you can then see that there's several different types of target that you can select. Choose Internet gateway. click on save routes and we now have our route successfully added.

![save](/pic/11.png)

![added](/pic/12.png)


We now have routing working for the instances that we deploy into our public subnets. Our private subnet is also associated with this Internet gateway. But even if we launched an instance there now, it wouldn't have a public IP, it wouldn't be able to access the outside world.


We do want to create a separate route table for the private subnets.

Choose create route table.

![creater](/pic/13.png)


![private](/pic/14.png)

I'm going to call this Private-RT, choose the VPC, and then click on create.

But what I want to do is associate the correct subnets.

![correct](/pic/15.png)


It doesn't actually show me the names of the subnets here, but if I just go back to my address blocks I know that the fourth, fifth, and sixth address blocks are the ones that I assigned to my private VPCs. Thats the 48, the 64, and the 80.

![blocks](/pic/16.png)

So let's come back over, choose edit subnet associations.
I want to select the 48, the 64, and the 80 and click on save.

![edit sb](/pic/17.png)

We can see that we have those subnets associated with the route table.

![assoc](/pic/18.png)

If we go back to the route table, which is associated with the public subnets, we can now see it only has these three and they're not associated. But because this is the main route table for the VPC, if you don't associate a VPC, it will always be associated with this route table. So that's it. We've now set up our VPC. The public subnets will have a route to that Internet gateway in their route table, and the private subnets have been separated so that they don't have any routes to the outside world. 
