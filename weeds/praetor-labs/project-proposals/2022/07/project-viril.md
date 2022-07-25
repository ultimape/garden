
# Project Viril \*

 ***Exploring tools for low-cost at-home open source male fertility testing.***

*Thanks for [helping us name it!](https://twitter.com/ultimape/status/1542462909325901824)

## Project Overview

A project proposal to address COVID related health concerns about male fertility.

### People

Nicholas _“@ultimape”_ Perry  (Project Lead)
* Vermont Technical College, Computer Engineering, Bachelor Of Science\* ‘09
* Social: [LinkedIn](https://www.linkedin.com/in/ultimape/), [Twitter](https://twitter.com/ultimape)

Norman Reed (Collaborator)
* Vermont Technical College, Software Engineering, Associate Of Science ‘09
* Social: [LinkedIn](https://www.linkedin.com/in/nsreed/)

\* (completed coursework and met graduation requirements, but no diploma due to outstanding debt)

### Organization

***Praetor Labs***

Nicholas and Norman have been working together informally under this banner since ~2012.  
Acting as a [semi-professional research and development firm](https://twitter.com/ultimape/status/567415183094861826) / “[hackerbase](https://wiki.hackerspaces.org/hackbase)” and live-in incubator, located in South Burlington, VT, USA. We consider ourselves co-founders.

Though this is not (yet) a registered business entity, the intention has been to formalize it as a Vermont [L3C corporation](https://en.wikipedia.org/wiki/Low-profit_limited_liability_company) upon releasing successful projects. @ultimape’s health issues drove challenges here so the initiative had been on hiatus until recently. At the moment we are acting as sole proprietors until we have the resources to make it official.

We are NOT affiliated with Vermont Technical College, nor are we associated with any other organization mentioned within this proposal.

### Project Summary

Our goal is to enable large scale [Citizen Science](https://en.wikipedia.org/wiki/Citizen_science) approaches for fertility related concerns, with a focus on issues associated COVID/Long-COVID. **We plan to iterate toward building & releasing open source designs for accessible low cost semen measurement.** Along with this, we hope to build out a related simple software framework to provide features amenable toward either [Quantified Self](https://en.wikipedia.org/wiki/Quantified_self) use cases, as well as facilitating [DIY healthcare](https://www.tedmed.com/talks/show?id=527608) initiatives to interface with local healthcare providers.

We will be leveraging the existing ecosystem of [DIY biology](https://en.wikipedia.org/wiki/Do-it-yourself_biology), cheap [FOSS](https://en.wikipedia.org/wiki/Free_and_open-source_software) compatible hardware, and the wide selection of low-cost cameras on the market. We plan to build and test competing options (including some exotic ones), then modify designs to simplify the sampling process.

To enable the secondary goal of integrating this data with existing health ecosystems, we plan to create a liberally licensed **software based testing framework to automatically assay fertility related metrics**, as well as ingest them into any [FHIR](https://en.wikipedia.org/wiki/Fast_Healthcare_Interoperability_Resources) compatible system (such as those offered by [GNU health](https://en.wikipedia.org/wiki/GNU_Health)).

Our hope is to also use this project as an opportunity to flesh out “Praetor Labs” to be more capable in working on further health related Quantified Self + Citizen Science ideas in the future.

## Background

### Problem

As the COVID-19 pandemic has unfolded, a small number of studies have come out detailing fertility related issues. There are concerns about [COVID associated Erectile Dysfunction](https://onlinelibrary.wiley.com/doi/10.1111/andr.13003) that have obvious implications toward fertility, but another issue that hasn’t seen a lot of press attention is the impact of COVID-19 on Sperm Health & [Semen Quality](https://en.wikipedia.org/wiki/Semen_quality).

There was a ‘prospective cohort study’ on 120 Belgian men to measure SARS-COV-2 in sperm, as well as take measurements of sperm quality. While they found no evidence of SARS-COV-2 in the semen, they still had an alarming conclusion:

> “However, couples with a desire for pregnancy should be warned that **sperm quality after COVID-19 infection can be suboptimal.** The estimated recovery time is 3 months, but further follow-up studies are under way to confirm this and to determine if permanent damage occurred in a minority of men.”
> 
> — [Sperm quality and absence of SARS-CoV-2 RNA in semen after COVID-19 infection: a prospective, observational study and validation of the SpermCOVID test](https://pubmed.ncbi.nlm.nih.gov/34937665/) [Dec 20, 2021]

With these findings echoing other smaller scale study on 22 infected individuals:

> “significant impairment in testicular histology was observed, including morphological changes in Sertoli cells, loss and sloughing into the lumen of tubular cells, reduction in the number of Leydig cells, and mild lymphocytic inflammation, suggesting testicular injuries during the disease course. Hence, **it appears that COVID-19 can adversely affect the male reproduction system.** However, to date, the consequences of COVID-19 on sperm parameters remain barely investigated.”
> 
> — [Semen parameters in men recovered from COVID-19](https://pubmed.ncbi.nlm.nih.gov/33975987/) [May 11, 2021]

Which all appear to reinforce findings around when issue was brought to light in this study on 183 covid positive individuals:

> “Lastly, we did not have [semen analysis] information from men prior to their SARS-CoV-2 infection, so **comparison with their baseline was not possible**; and without a known timeline for sperm production following an acute illness – the timing of our follow-up [semen analysis] was based off of the spermatogenesis timeline and the specific recovery time is still unknown for SARS-CoV-2 infection. Future studies will evaluate long-term impacts of SARS-CoV-2 diagnosis on sperm parameters and the hypothalamicpituitary-gonadal axis axis of men.”
> 
> — [Evaluation of SARS-CoV-2 in Human Semen and Effect on Total Sperm Number: A Prospective Observational Study](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8255403/) [Feb 24, 2021]

Above we quote that last study explicitly because it highlights a major oversight in virus related health monitoring. Upon investigating these topics further, one key thing stood out among all of them: These all are [prospective cohort studies](https://en.wikipedia.org/wiki/Prospective_cohort_study), with **no preexisting semen viability metrics from before infection**. This is unfortunate because those who have been shown to be particularly impacted by covid are also populations who are already at risk of fertility related issues.

Notably, there are currently no known direct causes to account for all these fertility challenges. Findings so far suggest the impacts of COVID may depend on underlying health factors that revolve around ACE2 and TRPMSS2 pathways.

> “Whether these symptoms arise from the direct effects of SARS-CoV-2 on the testis or whether they are mediated by other mechanisms, such as immune response and inflammation, remains to be determined; alternatively, underlying risk factors for SARS-CoV-2 could even be the ultimate cause. It is essential to remember that any disease state or environmental factor that dysregulates furin, ACE2 or TMPRSS2 can affect SARS-CoV-2 infection and its intersection with male fertility. The primary proteins involved in SARS-CoV-2 pathology, particularly ACE2 and TMPRSS2, are expressed throughout the body, including in the testis. **Thus, their infection or negative inhibition by SARS-CoV-2 could have severe consequences for male fertility.**”  
> 
> — [Implications of testicular ACE2 and the renin–angiotensin system for SARS-CoV-2 on testis function](https://www.nature.com/articles/s41585-021-00542-5) [Nov 26, 2021]

When considering these facets together with the sheer lack of data to create strong models, it raises troubling questions on the actual impact of the disease. It leaves a big unknown as to who is susceptible to these effects, and more questions than answers for why these fertility problems may be arising to begin with.

The latest study we found (at the time of writing this proposal) was from April 2022 and it's the same kind of pilot study as many of the others, still with conclusions similar to ~"well it might cause problems, we just don't know, need more data!".

> "These findings suggest that SARS-CoV-2 has direct or indirect effects on male reproductive health that linger after recovery, the researchers say. The work might also reveal insights into the pathophysiology of human reproduction in recovered men, they add. However, **they note that larger studies should be done to confirm these findings**, and a control group of men who recently recovered from other flu-like illnesses should be included to ensure that the findings are specific for COVID-19."
> — [COVID-19 alters levels of fertility-related proteins in men, study suggests](https://www.sciencedaily.com/releases/2022/04/220407141845.htm) [Apr 7, 2022]

This lack of useful data here is what we want to try to help solve.

### Opportunity

We feel this ***highlights a need for large scale measuring of sperm health related factors***, in particular for monitoring and targeting interventions to aid in recovery. It also shines a light on a need for ***gathering this kind of data to preempt this problem in future health crises***. Given the findings cited above, and [the role ACE related pathways play in fertility](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3821659/), it seems obvious to us that we need enhanced visibility around [the impact of this COVID on these pathways](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7778857/).

Short of people volunteering to intentionally infect themselves to get accurate before-and-after data about sperm health, **there is little that can be done to illuminate this space better than doing this kind of persistent widespread sampling** on a continuous basis for large swaths of the population.

Enabling more people to be involved with fertility related self-measurement, and accelerating contributions to our collective knowledge about these fertility concerns (and doing so at scale) is what we hope to achieve.

### Existing Solutions

In a western healthcare setting, semen analysis is typically done [largely via a manual process](https://www.healthline.com/health/semen-analysis), with a trained lab tech visually inspecting a semen sample under a microscope and performing various chemical tests. This leads to an essential bottleneck if we were to scale up testing to a large number of people. And costs can range anywhere from $50 to $300 PER SAMPLE depending on how many details are extracted.

Complicating matters is the need to ship live semen to the lab if not done in person at a fertility clinic. This often requires a simplified cryogenic cooling system similar to what is needed for sperm donation. [They have been known to explode](https://www.tampabay.com/news/publicsafety/calling-the-pasco-county-sheriffs-office-for-help-leads-to-social-media/2324584/). There are also challenges of not wanting to masturbate in a clinical setting due to embarrassment or the physical challenge of it. Apparently [hospitals buy sperm donor machines that cost upwards of $13,000](https://interestingengineering.com/sperm-extractor-now-used-in-clinical-settings-goes-viral) to get around this problem.

Tests are mostly done for the interests of having a child, so it is unlikely that insurance would cover testing semen for other health reasons, such as proxy measures for immune function or long term personal health studies.

Typical home semen analysis tools are often expensive or done with proprietary sampling chambers. There are entirely at-home kits that are able to measure many semen parameters, but they cost [upwards of $200](https://natalist.com/products/at-home-sperm-analysis). Other options include mail-in sperm count and videos and measurement of motility, but they [still cost $89](https://www.mylabbox.com/product/at-home-male-fertility-test/). All of these are limited to a small number of samples and aren’t meant for extended long term tracking.

### Where We are Different

What makes our approach special is the focus on creating a cheap reusable DIY tool that allows for sperm related metrics to be tracked both more regularly and at scale. This is not meant to replace existing solutions, but intended to augment existing semen measurement systems and fill in the gaps where it is currently lacking. The hope is this will be giving an option to those who are largely outside of the existing medical system.

By working with cheap and open hardware, people will be able to build their own for their home use, or provide access to fertility testing in remote hospitals operating under tight budget constraints.

It also means that the intimate act of collection can be performed at home without embarrassment, and without needing to jump through all the hoops that currently exist to get through the medical system. And in the case of using something like a Raspberry PI to do testing, there is also no need to submit your personal health details to some random app on your cellphone to be transmitted to who knows where.

As the limitations on cost and access are removed, this should enable a more cohesive monitoring and grassroots/citizen led protection of fertility health. Hopefully leading to better quantification of data that the current fertility treatment ecosystem [is not prepared to handle](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8818556/).

Further, being an open platform, it has the potential to be modified and extended in the future, with contributions from the global community allowing for it to be improved over time. For example, we would hope to see this expanding to handle other bodily fluids and non visual modes of sampling (such as PH, viscosity etc), microfluidics, and hopefully more.

## Project Details

### Methods

Praetor Labs currently provides enough space to host up to 4 people working at a time. It has room and tools to operate basic electronics development, access to a barebone 3d printer, and a large enough area to create a productive office space.

We will be utilizing local makerspaces in the Burlington area ( “[Generator](https://generatorvt.com/)” & possibly “[Laboratory B](https://www.laboratoryb.org/)” ) to access hardware and tools that we currently do not have on hand.

If tiers above the minimal one are approved, we also have extra space available to set up any additional rapid prototyping hardware on site.

### Direction & Aim

The plan is to select a wide array of cheap DIY microscopes and go about building them to evaluate their ease of use and judge their application toward quick and effective testing.

We will take design elements that we like and build something more specifically tailored for the need of semen sampling.

Simultaneously we will work on code to handle the data-flow from the microscope to various processing stages and then lastly conversion to the healthcare record format for ingestion and displaying the output.

We will continue to iterate on features until a reasonably useful semen sampling tool is produced.

Once we have a candidate microscope designed and ready to be tested, we will also start to work with taking samples. We will begin to gather training data for machine learning, as well as iterate over basic machine vision techniques for essential metrics. The initial goals include a rough semen count and a basic measure of motility. We will also be evaluating existing software packages tailored toward biology analysis to see if they might provide features we can leverage.

We are also aiming to create a way to facilitate a larger community to collaborate around the project. The shape this takes will evolve as the project unfolds, but will likely include a website to act as community portal, code sharing a la gitlab/github, along with video instructions for assembly and use.

We are looking at possibly creating other materials such as live streaming coding sessions and creating more visibility on the development process to act toward promoting the project. One interesting expansion here would be integrating with Quantified Self initiatives such as [OpenHumans](https://www.openhumans.org/about/) and other similar [Citizen Science research](https://www.nature.com/articles/d41586-018-07106-5) initiatives in the space of [Open Data](https://towardsdatascience.com/data-science-for-social-good-best-sources-for-free-open-data-5120070caf02).

If we are successful, we would like to expand Praetor Labs to go beyond semen tracking tools, branching out into a suite of services that contributes to a collaborative repository of health tracking data, either collaborating with or working under the same ethos as some other [useful public data repositories](https://publiclab.org/).

### Our Approach

Our hope with doing design iterations is that we will be able to make a version of the device that won't require a lot of experience to build. An ideal outcome with this project is to get one simple enough project that makerspaces and schools would be able to use them as introductory builds to teach with.

Our target would be someone who had basic soldering skills, could plug wires into a prototyping board following instructions, has some basic soldering experince, and if needed could cut out templates using a hacksaw. These are skills that can be gathered over a weekend by a dedicated individual, or picked up at a workshop at a makerspace in a couple of sessions.

While we will be using rapid-prototyping tools to work on development and design, it is not intended to have it be a requirement to build the end product. We want something a curious tinkerer could assemble over the weekend on a shoestring budget without needing any of the typical hobbyist tools. If it requires someone to buy a $170 3d printer to produce, we'd consider it a failure on our part.

We may end up producing multiple different versions with that self-build-ability in mind. While self-built ones would be valuable, we also see a potential need to produce them in small batches if the product is taken up by communities seeking to do their own research, or if an open-science friendly academic research wanted to formally use it in a study.

Aside from the direct production by local communities (which is what we plan on focusing on), there a larger game at play behind this. Basically we would be acting to prove that there is a demand for a tool like this, much like how one might use a Kickstarter as a form of social proof to get investors.

We are approaching this as an open source DIY project design because our hope is get enough initial interest by the maker community to get the idea out in the world. If in the future we can use that momentum to get something that a company, who has experience with pushing products thru medical licensing pipelines, would want to pick up the idea? That would be fine by us.

There is an existing model we are emulating that went from a DIY route to offerings backed by legitimate medical device providers. The [DIY insulin pump community](https://khn.org/news/diy-tech-gives-people-more-freedom-in-managing-diabetes/) is a big inspiration for us  and we had been happy to see something inspired by them them manage to [get a version of it passed thru the FDA's pipeline](https://www.fda.gov/news-events/press-announcements/fda-authorizes-first-interoperable-insulin-pump-intended-allow-patients-customize-treatment-through).

While our success is not dependent on a larger company backing us or our design to get it to market officially, this is something we are open to as a possibility.

### What about the FDA?

As far as we can tell, the tools we've mentioned are only qualified as medical devices as long as they are sold or offered for use within the United States.

However, as discussed above in "Our Approach" our goal is to simply provide instructions and schematics for the assembly of DIY microscopes and analysis software source code. We will not be selling the kits nor finished products, so our output should not fall under the purview of the [FDA's laws and regulations pertaining to medical devices](https://www.fda.gov/industry/regulated-products/medical-device-overview#Is%20my%20product%20a%20medical%20device)

In the end we are just designing free & open tools to aid in research and exploration.

### Product Costs?

Eyeballing the existing DIY microscopes available, we don't expect more than $200 for one has everything to do it standalone. The prototype here would be a raspberry PI to run the machine vision code.

This is not an ideal price point and we intend to try to get the basic computer vision one going on cheaper hardware (e.g. the Raspberry PI Zero or a BeagleBone) by trading off time for cost. That may end up around $60.

We also found a couple that use Arduinos and would have the end user run the image analysis on something like their phone or a desktop/laptop they already have. These can be quite cheap if you are willing to pair them down to just [the microchip and basic power circuit](https://www.instructables.com/DIY-Microscope/). We'll be designing the scanning software to support externally sourced images for that reason. Our hope is to provide flexibility so that communities and individuals can figure out solutions that work for their needs.

The initial phase of of this initiative is specifically around exploring this space more and getting a better sense of what it would actually take to build existing solutions. We'll have a better sense of what the final costs will be once we are thru that step.

### What about selling them?

We are quite hesitant to put them up for sale, even at cost, because **that would put us into the medical device manufacturing area** and under the jurisdiction of the FDA.

Giving them away as part of a donation backed program might be viable, but its not something either of us personally have the skills or chutzpah to spearhead. A hope with being an open project that someone else may be willing to take up that flag and run with it.

We should be able to **give away prototype units away to help gather initial samples, but they would not be complete devices**, nor would they be intended to be used in anything more than a data gathering initiative to train our Machine Learning modules with. With the Dream Tier Budget (see the "Budget" section below) we will be able to do small scale manufacturing in house without much problem. It is also something we plan on doing in some of the other tiers to get data faster, but how many we can make it depends on how much of the microscopes parts we are able to reuse.

To really nail the larger problem space we need something that is more dispersed and decentralized. So **we'd rather help others develop this on their own or as grass-roots community initiative**. This gives us potential for more genuine training data while also explicitly encouraging adoption among those who would need the tool the most.

There is definitely room in the future to transition toward an at-cost sales model once we've got enough of a ground swell to be able to support it as a community but this isn't something that would be worth targeting until the product has shown successes in the small scale and we have a hope of getting it pushed thru the FDA's gauntlet. Until then we are happy to simply provide plans and develop it into something that can help people DIY. Even if we head down this route, **We will always publish everything we design related to this project as free & open source.**

### Crowdsourced Image Tagging & other Training Data?

Like most machine learning projects, we will need a means to acquire accurately tagged training data to get appropriately trained inferences and image detection.

The legal limitations here mean literally being unable to buy semen, not being able to act a medical establishment, fertility clinic, or sperm bank. We also do not wish to present ourselves as providing or selling a service, nor as a medical device supplier. But as maker of a DIY tool.

So to actively procure samples ourselves we would need to do it in a way that any provided test kits are strictly used a volunteer basis to contribute to the product.

If someone wants to build their own device and contributes their image data to us to develop the ML model, that doesn't seem to be an issue, so we intend to go this route.

We do not yet have a concrete plan on how to get the labeling, but it will likely be some kind of small game on the community site. Praetor Labs has spent some time working on crowdsourcing back in 2010 and believe we could manage to get a basic system off the ground to gamify tagging somehow.

Until we have our own tests with the microscopes to work with, we don't exactly know what we may need here, so the specifics of how this happens is not set in stone. But we will be focused on minimizing what we collect and setting up data handling & safety standards that we both feel comfortable with.

For the sake of being open with potential biases in the training data, we hope to present some kind of dashboard that logs metrics about training completeness in terms of health issues and stuff like ethnicities and other demographic data if they are provided.
### Notes on Data Security

#### Security in the Final Product

We would be structuring the designs as a sort of standalone appliance. For a typical end user, the default mode here is going to be entirely offline, private and on device.

For sharing data with others, we intend for it to be using an SD card or thumbdrive by default, with the user explicitly selecting the export location and generating the FHIR data to import elsewhere. We envision it be similar to how one needs to bring in a blood sugar testing tool or CPAP memory logging card to have the data ingested by a doctor. Another option may be to have it always write FHIR data to an SD card like how some CPAP machines do. How it will actually work will depend on usability testing.

The default FHIR option would not include any image data, and the default operation will likely be to delete the photos after being displayed and processed. Longer term saving to the device will be an option for the more curious user, but will not be on by default. For many, just the count and rough motility metric will be enough. Tho they may want to be able re-evaluate the samples with future analysis packages released down the road.

What happens when a user transmits these to a research or medical professional is outside of our control, so we will hopefully be providing recommendations for secure storage of this data. We will be largely depending on existing medical data security standards and laws that exist for medical institutions.

#### Security During Development & With Training

We are actively exploring options for securely transmitting data to us ala something like a dropbox system. This is a problem that I would like to solve as part of the course of this project.

Once acquired, images used in our machine learning project will stay encrypted on a couple of hard-drives in a secured location (literally a safe) and only transferred to the Machine Learning device during the training phases.

Part of the idea of having standalone machine learning computers is to have them air gapped during training.

During the training phase, we would be providing a place to upload semen photos and short video content on an opt in and volunteer basis. It would be similar to the process of exporting data to a FHIR format to send you a clinician, but instead of some JSON files, it would be a small bundle of photos and possibly some metadata that the user selects and embedded in an appropriate compressed file.

This would be considered an advanced feature that the typical user would not need to use, but would be there for people who want to contribute this data during the initial development of the models.

We would provide a series of downloadable configuration files on a GitHub/Gitlab/GiTea (or equivalent) repo that would present survey questions to the user and reconfigure things like resolution and capture time. When the configurations are present, there would be some advanced settings visible that let you preserve and bundle up the photos themselves along with metadata for whatever questions we have at the time.

By default the software will not present a way to export images unless these configuration files are present, and the way they get on to the device will be largely manual and have consent notifications both in the app and on the website. This will ensure that the end user has intended to use these features and is structurally closed down by the nature of not ever having these configs to begin with in the default product.
#### Long term Training Data Security

What ends up happening with the photos once we gather them is not yet entirely clear. We do not plan, nor want to store this data for the long term, but would be willing to work with other initiatives who would be interested in access to this data (with explicit consent by the people who submit it). And it would be nice to be able to have others build on the machine learning models.

We would like to provide an option to include provided images in a public dataset and share them on some kind of open-data science repository, but understand that may limit the training set we can gather. If we provide that option, we would set up steps to ensure that data is not mishandled, with physically isolating the datasets and presenting these options clearly upon uploading (and via the configuration option discussed above). We will be treating it like one would treat any medical data.

Both of us are very keen on how data is handled and will not do something that goes against our ethics. The last thing we want is to have someone's private medical data leaked because it was stuck up on a cloud service somewhere by someone who didn't respect it or understand what they were doing.

We will report any breeches of this in a visible place to make sure that people are kept abreast of any concerns.

#### Security with Crowdsourcing Visual Tagging

For content tagging it would be necessary to present the picture content to others people to mark up. We will be extending the configuration system discussed above so that the typical user will have to go out of their way to share their data to do this. The configuration files will be designed to present additional consent notifications if any other human being will be looking at the content for scoring purposes.

A separate export file with all personally identifiable meta data explicitly expunged will be produced, to be handled separately from the training workflow. Tools will be built on our end to automate the inspection of these files to ensure relevant metadata is either present or missing.

### Budget

We’ve broken down our project into 4 different tiers that reflect varying levels of capabilities, balancing speed of delivery and quality improvement with respect to costs.

We should **be able to meet the listed outcomes within 6 months**.

However we have also provided the costs for a year. More time on the project will allow for increasingly thorough semen analysis coverage. Time will also provide a runway to gather larger quantities of data for machine learning. A year would also give more time to dedicate toward fostering a community, allowing us to focus on promotion and support needs and ensure the project will flourish.

Not listed explicitly below is the option to pay for additional people to live at Praetor Labs for the duration of the project. We would find it useful to hire someone local to work/live with us while developing the machine learning side of things. We live in an area with multiple nearby universities, and have contacts at both UVM & VTC who would love to connect a recent graduate with a resume building opportunity like this.

----

#### Low Tier: Low Cost / Minimum Viable Solution  

~ $50k for 6 months.

This tier covers the basics needed for testing multiple DIY microscopes and iterating on a design to make a simple semen sampler capable of producing some useful fertility metrics.

**Outcome:**

1.  We will provide instructions for people to build a device capable of recording their sperm.
    
2.  We will publish a GNU Health compatible software package, enabling automation of basic semen analysis using simple computer vision techniques.
    

**Limitations:**

-   At this tier, any machine learning will necessarily be entirely crowdsourced.
    
-   Will require others to build out copies of the hardware on their own to gather general assistance by a larger community to provide enough data to help fill out the Machine Learning based automation. This may slow initial adoption of the project.
    

This funding tier involves working out of local hackerspaces to access some tools that we do not have at Praetor Labs, so we will be challenged by scheduling constraints and traveling concerns, which will slow down our velocity.

| Item | Per Person | 1 Month | 6 Months | 1 Year | 
| ---- | ---------- | ------- | -------- | ------ | 
| People (x2) | $1,543.00 | $3,086.00 | $18,516.00 | $37,032.00 | 
| Facility Costs (flat) |  | $2,320.00 | $13,920.00 | $27,840.00 | 
| Services (x2) | $185.00 | $370.00 | $2,220.00 | $4,440.00 |
| Non-Deductible Tax |  | $1,612.95 | $9,677.69 | $19,355.38 | 
| ***Subtotal*** |  | $7,388.95 | $44,333.69 | $88,667.38 | 
| Supplies & Parts |  | $2,466.00 | $2,466.00 | $2,466.00 | 
| Tooling & Hardware | | $1,917.00 |  $1,917.00 | $1,917.00 | 
| VT sales Tax (7%) |  | $306.81  | $306.81 | $306.81 | 
| ***Subtotal***  |  |  $4,689.81 |  $4,689.81 |  $4,689.81 | 
| ****Total***  |  | $12,078.76 | **$49,023.50**  | $93,357.19 | 

----

#### Middle Tier: Faster Development  

~ $55k for 6 months.

Everything in “Low Tier” as well as attempting to make additional & more complicated microscope systems to compare against (including a holographic one).  
  
This tier includes an assortment of basic hardware (and a dedicated computer to control them) for Praetor Labs. This will enable us to work outside of the limitations of the local hackerspace scheduling, thus speeding up production time and allowing for more aggressive design iteration.

**Outcome:**

1.  We will provide instructions for people to build a device capable of recording their sperm.
    
2.  We will publish a GNU Health compatible software package, enabling automation of basic semen analysis using simple computer vision techniques.
    
3.  Faster iteration means we will get to a viable product sooner.
    

**Limitations:**

-   At this tier, any machine learning will likely be entirely crowdsourced.
    
-   Will require others to build out copies of the hardware on their own to gather general assistance by a larger community to provide enough data to help fill out the Machine Learning based automation. This may slow initial adoption of the project.
    
| Item | Per Person | 1 Month | 6 Months | 1 Year | 
| ---- | ---------- | ------- | -------- | ------ | 
| People (x2) | $1,543.00 | $3,086.00 | $18,516.00 | $37,032.00 | 
| Facility Costs (flat) |    | $2,320.00 | $13,920.00 | $27,840.00 |
| Services (x2) | $185.00 | $370.00 | $2,220.00 | $4,440.00 | 
| Non-Deductible Tax |    | $1,612.95 | $9,677.69 | $19,355.38 | 
| ***Subtotal*** |    | $7,388.95 | $44,333.69 | $88,667.38 | 
| Supplies & Parts |    | $4,039.00 | $4,039.00 | $4,039.00 | 
| Tooling & Hardware |    | $5,267.00 | $5,267.00 | $5,267.00 | 
| VT sales Tax (7%) |    | $651.42 | $651.42 | $651.42 | 
| ***Subtotal*** |    | $9,957.42 | $9,957.42 | $9,957.42 | 
| ***Total*** |    | $17,346.37 | **$54,291.11** | $98,624.80 | 

----

#### High Tier: Advanced Automation  

~ 66k for 6 months.

Everything in the “Middle Tier” as well as scaling up our own creation of sample data and being able to make a serious attempt at rolling out some machine learning on our own.

This tier includes hardware intended to be used for machine learning model generation, as well as more materials to make multiple devices for faster sample gathering.

**Outcome:**

1.  We will provide instructions for people to build a device capable of recording their sperm.
    
2.  We will publish a GNU Health compatible software package, enabling automation of basic semen analysis using simple computer vision techniques.
    
3.  Faster iteration means we will get to a viable product sooner.
    
4.  Provide for primitive Machine Learning identification on a limited sample set.
    

**Limitations:**

-   Will require others to build out copies of the hardware on their own to gather more training data.
    
-   Will need to crowdsource labeling of data.
    
-   Due to legal limitations we may not be able to access large quantities of human semen samples for testing with.

| Item | Per Person | 1 Month | 6 Months | 1 Year | 
| ---- | ---------- |  ------ |  ------- | ------ | 
| People (x2) | $1,543.00 | $3,086.00 | $18,516.00 | $37,032.00 | 
| Facility Costs (flat) |    | $2,320.00 | $13,920.00 | $27,840.00 | 
| Services (x2) | $185.00 | $370.00 | $2,220.00 | $4,440.00 | 
| Non-Deductible Tax |    | $1,612.95 | $9,677.69 | $19,355.38 | 
| ***Subtotal*** |    | $7,388.95 | $44,333.69 | $88,667.38 | 
| Supplies & Parts |    | $7,259.00 | $7,259.00 | $7,259.00 | 
| Tooling & Hardware |    | $12,552.00 | $12,552.00 | $12,552.00 | 
| VT sales Tax (7%) |    | $1,386.77 | $1,386.77 | $1,386.77 | 
| ***Subtotal*** |    | $21,197.77 | $21,197.77 | $21,197.77 | 
| ***Total*** |    | $28,586.72 | **$65,531.46** | $109,865.15 | 

----

#### Dream Tier: Enabling Scale  

~ $80k for 6 months.

Not only do we do everything in the “High Tier” at a larger scale, we will also be able to personally produce multiple units to send to other places and seed the use of the tool at interested hospitals / communities who would be interested in providing training data.

This tier provides more capable machine learning hardware to iterate faster on the model generation. It also includes a laser cutter which will provide us with around the clock access to a suitable tool for small scale production, and more resources for building them.

**Outcome:**

1.  We will provide instructions for people to build a device capable of recording their sperm.
    
2.  We will publish a GNU Health compatible software package, enabling automation of basic semen analysis using simple computer vision techniques.
    
3.  Faster iteration means we will get to a viable product sooner.
    
4.  Producing a large number of prototypes will provide a means to more rapidly gather training data.
    
5.  Machine learning models will be more robust and we should be able to measure more sperm parameters.
    

**Limitations:**

-   While lessened to some degree, we may still require others to build out copies of the hardware on their own to gather more training data.
    
-   Will need to crowdsource labeling of data.
    
-   While somewhat mitigated by providing pre-built devices, due to legal limitations we may not be able to access large quantities of human semen samples for testing with.

| Item | Per Person | 1 Month | 6 Months | 1 Year | 
| ---- | ---------- | ------- | -------- | -------| 
| People (x2) | $1,543.00 | $3,086.00 | $18,516.00 | $37,032.00 | 
| Facility Costs (flat) |    | $2,320.00 | $13,920.00 | $27,840.00 | 
| Services (x2) | $185.00 | $370.00 | $2,220.00 | $4,440.00 | 
| Non-Deductible Tax |    | $1,612.95 | $9,677.69 | $19,355.38 | 
| ***Subtotal*** |    | $7,388.95 | $44,333.69 | $88,667.38 | 
| Supplies & Parts |    | $15,788.00 | $15,788.00 | $15,788.00 | 
| Tooling & Hardware |    | $26,394.00 | $26,394.00 | $26,394.00 | 
| VT sales Tax (7%) |    | $2,952.74 | $2,952.74 | $2,952.74 | 
| ***Subtotal*** |    | $45,134.74 | $45,134.74 | $45,134.74 | 
| ***Total*** |    | $52,523.69 | **$89,468.43** | $133,802.12 | 

----

## Project Outputs

### Mission

We believe our proposal is a high-value COVID project that targets a need that traditional funding sources are neglecting because there’s no financial incentive. Being open source and low-cost it is implicitly accessible to low-income countries, while also potentially providing tools to the global community where they are needed. And we fully intend to work within a FOSS mindset and use open standards to enable as wide of a reach as possible.

### Other Potential Impacts

There are other somewhat obvious ways that large scale semen quality testing could have an impact beyond just monitoring for COVID related issues.

While at first glance one might think fertility concerns appear to be limited to [the United States](https://twitter.com/elonmusk/status/1529097667858546689), it would seem we are currently experiencing a [global fertility decline](https://www.bbc.com/news/health-53409521) with nearly all countries experiencing [drastic changes in their birth rate](https://www.visualcapitalist.com/cp/charted-the-global-decline-of-fertility-rates/) across the board. The challenges we face here as species are myriad, and it is likely to be a complex and multifaceted issue. Challenges here may be stemming from any number of problems: from [pesticide use](https://www.hsph.harvard.edu/news/hsph-in-the-news/pesticides-produce-fertility-women/), growth of the [pharmaceutical use](https://modernfertility.com/blog/antidepressants-and-trying-to-conceive/) for various conditions, general [disruption of the microbiome](https://mefj.springeropen.com/articles/10.1186/s43043-021-00078-z), concerning findings around [“Forever Chemicals”](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8038605/) and [Microplastics](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7967748/), as well as any number of other [environmental contaminants](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6396757/), or even just large scale shifts in [dietary factors](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6079277/).

Having widely available access to individualized at-home semen quality measurement would help us collectively learn more about the particular issues we are facing. Further, if we can eventually pool these insights together, humanity as a whole may be better able to uncover yet-unknown factors and work toward heading off future challenges before they become major concerns.

Another interesting impact is in the space of vaccine hesitancy. [There are](https://www.frontiersin.org/articles/10.3389/fvets.2021.761304/full)  [numerous](https://www.swineweb.com/researchers-evaluate-nanoparticles-to-protect-fertility-during-intrauterine-vaccination/)  [examples](https://pubmed.ncbi.nlm.nih.gov/21676477/) of [vaccines causing breeding problems](https://pubmed.ncbi.nlm.nih.gov/17952734/)  [among the field](https://www.ars.usda.gov/ARSUserFiles/30300500/Publications/Uploaded1272012/2018%20Bovine%20Pract%20J%2052%2053-58.pdf) of [animal husbandry](https://www.nifa.usda.gov/grants/programs/animal-reproduction), but this is also a space where we are actively monitoring their reproduction due to being raised as livestock and managed closely. This is not the case in humans. Having wide access to fertility testing tools at a community level may provide early alerts for [semen related vaccine complications](https://pubmed.ncbi.nlm.nih.gov/35713410/). More confidence here should aid directly in reducing future vaccine hesitancy. This would have been especially useful when vaccine development is accelerated and little to no fertility safety trials (animal or otherwise) were done and [had to be done independently, after the fact](https://jamanetwork.com/journals/jama/fullarticle/2781360).

And the flip side of this is we can also use these tools to aid in better family planning. Like knowing for certain how one’s body [responds to hot water](https://www.ucsf.edu/news/2007/03/97970/hot-tubs-hurt-fertility-ucsf-study-shows) and similar forms of [heat-based male birth control](https://en.wikipedia.org/wiki/Heat-based_contraception). Or [better target exotic future contraceptives](https://www.acs.org/content/acs/en/pressroom/newsreleases/2022/march/non-hormonal-pill-could-soon-expand-mens-birth-control-options.html) so they don’t result in [potentially disastrous side effects](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3279749/).

## More About the Team

#### Nicholas “@ultimape” Perry

@ultimape’s background in Computer Engineering means he has experience with low-level hardware development, having worked on circuit design and microcontroller programming on a large number of platforms.

His original expertise was in computer repair and troubleshooting and is experienced working in help-desk roles, and as an IT service provider. While working at an IT MSP ([managed service provider](https://en.wikipedia.org/wiki/Managed_services)), he was part of a team that supported a large number of clinics and became familiar with IT facing healthcare regulations such as [HIPAA](https://www.cdc.gov/phlp/publications/topic/hipaa.html). His focus was on setting up easy to use computer solutions, infrastructure maintenance automation, as well as providing teaching material to end users.

The past 7 years of his life have been obsessively spent trying to overcome a number of debilitating health issues, and in doing so he has sought to become an expert in areas related to his health. Artifacts from this process include twitter [threads that span literal years of research notes,](https://twitter.com/ultimape/status/713437626334912513) as well as a [massive healthcare research repository](https://www.are.na/self-exploration/index).

Because of [slipping through the cracks](https://twitter.com/ultimape/status/828739118368747522) of the medical system, he has a strong focus on [DIY healthcare](https://twitter.com/ultimape/status/918917956209397762) out of necessity, and is passionate about helping people gain access to tools and knowledge where it is fundamentally lacking.

With the interest in hardware and computation, his main focus is on using Quantified Self tools to track and measure health metrics to hone in on relevant factors. This focus has enabled him to perform a number of experiments, including a gut cleanse in 2018 that reversed his treatment resistant major depression. He has been IBD symptom free for 7 months as of writing this document.

From August 2019 to February 2021 he worked for a healthcare focused dual-entity under [the non-profit wing](https://selfresearch.org/) as a research consultant. This gave him an institutional backing to continue to explore his own health issues, while doing research supporting a framework tailored toward integrating Quantified Self data with the existing medical infrastructure. Due to funding issues and challenges with his health, the relationship ended in March 2021. He has been doing independent research on his own via donations on patreon.

As the pandemic unfolded, the direction of the firm pivoted toward a COVID health tracking tool and as such, an extensive amount of time was spent exploring the research as it came out. With many of his symptoms mirroring those of Long Covid, much of his focus was on figuring out a way to reverse them in himself. Knowing about the semen quality concern and [the connections between fertility and general health](https://www.are.na/ultimape/owo-my-balls-and-other-tales-on-semen-quality) is a direct result of this.

#### Norman Reed

Norman has over ten years experience as a software engineer, including five years of experience in the healthcare software industry. During this time he worked on Electronic Health Record (EHR) products, including desktop integration of medical devices, prescription management, UI/UX design, and developed a working knowledge of HL7 FHIR schemas.

Having known each other since their time in college, Norman has worked alongside @ultimape on a number of projects under the Praetor Labs initiative.

## Budget Detail

Below is the chart we used to calculate the costs of tools & supplies we intend to use to build the microscopes. The prices are rounded up and estimated based on perusing parts list of various options on the market and are vague because of not knowing exactly which set of parts we will end up buying multiple of. We have also baked in room for making mistakes - like accidentally destroying a camera during the “delidding” process that some of the holographic ones require.


 | Type | Minimal | Middle | High | Dream |
 | ---- | ------- | ------ | ---- | ----- |
 | Lab Tools                             | $300.00     | $2,315.00     | $9,265.00    | $19,090.00  |   
 | Bulk Material                         | $886.00     | $1,089.00     | $1,769.00    | $2,638.00   | 
 | Project Parts                         | $1,580.0    | $2,950.00     | 5,490.00     | $13,150.00  | 
 | Hardware                              | $1,817.00   | $3,152.00     | $3,287.00    | $7,304.00   |
 | VT sales Tax (7%)                     | $320.81     | $665.42       | $1,386.77    | $2,952.74   |
 | ***Subtotal*** | _$4,903.81_ |  _$10,171.42_ | _$21,197.77_ |_$45,134.74_ | 


 | Type | Item | Cost | Unit | Qty | Minimal | Qty | Middle | Qty | High | Qty | Dream |
 | ---- | ---- | ---- | ---- | --- | ------- | --- | ------ | --- | ---- | --- | ----- |
 |    | Number of scopes |    |    | 5 |    | 10 |    | 20 |    | 50 |    | 
 | Lab Tools | Resin Printer | $600.00 |    |    |    | 1 | $600.00 | 1 | $600.00 | 1 | $600.00 | 
 | Lab Tools | Desktop CNC Router | $600.00 |    |    |    | 1 | $600.00 | 1 | $600.00 | 1 | $600.00 | 
 | Lab Tools | Desktop Laser Cutter | $2,750.00 |    |    |    |    |    | 1 | $2,750.00 | 1 | $2,750.00 | 
 | Lab Tools | Lamda ML Workstation | $12,000.00 |    |    |    |    |    |    |    | 1 | $12,000.00 | 
 | Lab Tools | High End 3d Printer | $1,100.00 |    |    |    |    |    |    |    | 2 | $2,200.00 | 
 | Lab Tools | Lambda ML Laptop | $3,500.00 |    |    |    |    |    | 1 | $3,500.00 |    |    | 
 | Lab Tools | Medium Level 3d printer | $700.00 |    |    |    | 1 | $700.00 | 2 | $1,400.00 |    |    | 
 | Lab Tools | Pipette (adjustable) | $50.00 |    |    |    |    |    |    |    |    |    | 
 | Lab Tools | Additional Electronics Workbench | $525.00 |    |    |    |    |    |    |    | 1 | $525.00 | 
 | Lab Tools | Solder Station | $115.00 |    |    |    | 1 | $115.00 | 1 | $115.00 | 1 | $115.00 | 
 | Lab Tools | Incidental Tools | $300.00 |    | 1 | $300.00 | 1 | $300.00 | 1 | $300.00 | 1 | $300.00 | 
 | Project Parts | LED power | $20.00 |    | 6 | $120.00 | 11 | $220.00 | 21 | $420.00 | 51 | $1,020.00 | 
 | Project Parts | Microcontroller | $150.00 |    | 6 | $900.00 | 11 | $1,650.00 | 21 | $3,150.00 | 51 | $7,650.00 | 
 | Project Parts | Camera | $80.00 |    | 6 | $480.00 | 12 | $960.00 | 22 | $1,760.00 | 52 | $4,160.00 | 
 | Project Parts | Lenses | $20.00 | 4ct | 3 | $60.00 | 4 | $80.00 | 6 | $120.00 | 14 | $280.00 | 
 | Project Parts | Motors | $20.00 | 10ct | 1 | $20.00 | 2 | $40.00 | 2 | $40.00 | 2 | $40.00 | 
 | Hardware | Filament Storage & Accessories | $100.00 |    | 1 | $100.00 | 1 | $100.00 | 2 | $200.00 | 4 | $400.00 | 
 | Hardware | Computer | $1,200.00 |    | 1 | $1,200.00 |    |    |    |    | 1 | $1,200.00 | 
 | Hardware | 16 port Gigabit Switch | $67.00 |    | 1 | $67.00 | 1 | $67.00 | 1 | $67.00 | 2 | $134.00 | 
 | Hardware | Durable Computer | $2,500.00 |    |    |    | 1 | $2,500.00 | 1 | $2,500.00 | 2 | $5,000.00 | 
 | Hardware | 8 port Gigabit Switch | $35.00 |    |    |    | 1 | $35.00 | 2 | $70.00 | 2 | $70.00 | 
 | Hardware | Wireless Router | $200.00 | 1ct | 2 | $400.00 | 2 | $400.00 | 2 | $400.00 | 2 | $400.00 | 
 | Hardware | 5 port Gigabit Switch | $25.00 |    | 2 | $50.00 | 2 | $50.00 | 2 | $50.00 | 4 | $100.00 | 
 | Bulk Material | LEDs (point) | $9.00 | 10ct | 4 | $36.00 | 4 | $36.00 | 4 | $36.00 | 4 | $36.00 | 
 | Bulk Material | LEDs (5mm) | $7.00 | 25ct | 2 | $14.00 | 2 | $14.00 | 2 | $14.00 | 2 | $14.00 | 
 | Bulk Material | LEDs (circle) | $10.00 | 1ct | 1 | $10.00 | 2 | $20.00 | 5 | $50.00 | 10 | $100.00 | 
 | Bulk Material | Misc Electronics Components | $230.00 |    | 1 | $230.00 | 1 | $230.00 | 2 | $460.00 | 2 | $460.00 | 
 | Bulk Material | LEDs (Strip) | $20.00 | meter | 5 | $100.00 | 5 | $100.00 | 5 | $100.00 | 5 | $100.00 | 
 | Bulk Material | Fastener Kits | $15.00 |    | 6 | $90.00 | 6 | $90.00 | 6 | $90.00 | 6 | $90.00 | 
 | Bulk Material | Collection Containers | $7.00 | 50ct | 2 | $14.00 | 5 | $35.00 | 9 | $63.00 | 14 | $98.00 | 
 | Bulk Material | Sample Slides | $15.00 | (150ct | 6 | $90.00 | 10 | $150.00 | 16 | $240.00 | 24 | $360.00 | 
 | Bulk Material | 3D Printer Filament | $20.00 | kg | 10 | $200.00 | 15 | $300.00 | 25 | $500.00 | 55 | $1,100.00 | 
 | Bulk Material | Pipette Tips | $25.00 | 500ct | 2 | $50.00 | 2 | $50.00 | 4 | $100.00 | 4 | $100.00 | 
 | Bulk Material | Syringes | $12.00 | 20ct | 1 | $12.00 | 2 | $24.00 | 3 | $36.00 | 5 | $60.00 | 
 | Bulk Material | Shrinky Dink sheets | $20.00 | 50ct | 2 | $40.00 | 2 | $40.00 | 4 | $80.00 | 6 | $120.00 | 
 