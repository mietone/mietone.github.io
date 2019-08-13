---
layout: post
title:      "Redux Data Flow"
date:       2019-08-13 00:52:44 +0000
permalink:  redux_data_flow
---


Before jumping into Redux, I heard it was hard but oh boy! I didn’t think it would be this hard! There are lots of jargons to start with like actions, reducers, action creators, immutability and so on. Then when you follow a tutorial and start writing codes, you jump between multiple files and it’s really confusing. I think it’s important to understand the flow of data in Redux from a high level view at the beginning. Basically data flows one way in a predictable manner.

Action -> Reducer -> Updated State
