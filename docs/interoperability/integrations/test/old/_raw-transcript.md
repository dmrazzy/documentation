Dec 17, 2025

## Meeting Dec 17, 2025 at 17:13 GMT+05:30 - Transcript




### 00:00:00

   
**Keshav Singh:** Let's just talk in English then  
**Rachik Sharma:** I'm not sure.  
**Keshav Singh:** English  
**Rachik Sharma:** Uh, no problem. So, what we did? Uh, where was I? Uh, you want me to record the session or transcript is okay?  
**Keshav Singh:** record also. Can we do both at the same time?  
**Rachik Sharma:** Okay. So what what we did at that time was we uh uh we released this  
**Keshav Singh:** Okay.  
**Rachik Sharma:** uh uh platform to support the two major flows. One is child birth registration and one is child uh sorry one is child new birth registration and another one is uh any person who is deceased — basically death registration. Okay, so what does it mean is CRVS will collect the  
**Keshav Singh:** H.  
**Rachik Sharma:** national ID of the parent in case of child birth I'm saying it will collect the national ID of parent they will authenticate it against eSignet, make sure that the person is a legitimate citizen then they'll collect the details of the child they will verify everything that it's a legitimate birth.  


   
 

### 00:01:18

   
**Rachik Sharma:** So we are considering them as a source of truth. So this source of truth is one major point that has come across in many uh uh discussions and if you'll go through the documentation you will see that we have clearly noted it down on multiple locations saying that we do not do any de-duplication or data validation because we consider CRVS as a source of truth. Okay. So this is one major point that has to be there. So the idea is we consider them source of truth. We consider that they are doing their due diligence before sending us the request. Now once they send us the request what we have done for uh accepting their request is we have given them — we have opened  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** an API for them. And this is not only for them this is for any external partner.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Let's say tomorrow a passport service comes in and says that we want to uh send you the details of a person and we want you to uh create a UIN for that.  
   
 

### 00:02:07

   
**Rachik Sharma:** Okay. Or a driving license. Okay. any uh department which is external to MOSIP and wants to connect with MOSIP or access the MOSIP system we have given them a way of exposing this create packet API where they will act as a parallel to a registration client.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So currently packet is created by registration client but now we are saying — now we are saying that source  
**Keshav Singh:** CL  
**Rachik Sharma:** is different from registration client; source is CRVS in this case. Okay I'll share my screen one second so that I can show you what are the — okay so this is the whole idea. Similarly for death, what they were expecting was that we deactivate the ID but in MOSIP we do not consider deactivating the ID. I'll go into the details of why that is a decision made. But what we do there is we ask them to send the request for a deceased person with their  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** UIN and then they submit the request and then once it is processed within MOSIP we add a flag saying deceased = true.  
   
 

### 00:03:23

   
**Rachik Sharma:** UIN is still active. Okay. Uh if somebody is using that UIN and let's say somebody has — in my family and they have marked me as a nominee. Now I go to the bank to get all their savings. They will authenticate first using the ID of that person saying whether that is a legitimate bank account holder or not. So for that UIN is still activated because there are multiple services that can be tied to the same national ID of the deceased.  
**Keshav Singh:** Okay, got it.  
**Rachik Sharma:** But relying party will get a flag in a user info saying this is deceased = true. So that will confirm the point that the person has died. So relying party — and let's say somebody is trying to register for a new service using the same UIN — then also they will get a flag saying this is a deceased person's UIN. Okay. Now it is up to the relying party whether they want to give the service to the family or not based on the particular scenario.  
   
 

### 00:04:19

   
**Rachik Sharma:** So in MOSIP for death all we do is we add a flag.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Okay.  
**Keshav Singh:** Got it.  
**Rachik Sharma:** These were the two major flows that we supported. Okay. Now coming to the D. So if you'll go through the first page you'll see what is it how it works why it is meta general — theory about why such integration is needed, what are the principles, what are the objectives — those are written  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** here based on my understanding okay so this has to be overhauled — building a story saying why such a need arises, what are different scenarios that a child might be required  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** to register for a UIN at the time of birth. How it helps them. So basically by the time of birth you have a UIN that you can use for your lifelong services. Okay you want to get admission into school. You want to apply for child benefit services. The family wants to apply for child benefit services.  
   
 

### 00:05:24

   
**Rachik Sharma:** Now a child becomes adult — then they want UIN through for the whole life  
**Keshav Singh:** Yeah. Mhm.  
**Rachik Sharma:** cycle — you are sorted if you have a foundational ID at the time of birth. Okay. So this whole page is talking about similar things. Okay. Streamlining processes — that's the main point I was making. Secure and reliable data verification. Issuance of unique identity linked to birth certificates. Accuracy in identification. Now because at the very start of your life you are given an ID, the same ID can be maintained throughout. So similarly for deceased you can actually update the information for deceased when the person has died because that's the responsibility of CRVS. So we are also following the same. Okay. So this whole page is talking about those things. Okay. Once you go through it you'll get to know what we are trying to portray here.  
   
 

### 00:06:19

   
**Rachik Sharma:** Okay. But it is written from my perspective. And this page has not been called out for any issues and all but still if some update or overhaul is required here we'll have to do it. Yeah. Now coming to the scope. Now scope as I already told — birth and death. Yeah. For each — and there is one more scenario which is demographic data update. We added it here earlier but the clarity was not there. Okay. So now this also has to be updated. Now third scenario is let's say during the time of birth registration there was an issue with the name of the child or let's say when the child was registered there were parents, now parents have died and child is getting adopted by someone and the surname has changed or name has changed. Later it was identified that the date of birth in the birth certificate is not correct; they want to update that date of birth — so coming to the birth registration.  
   
 

### 00:07:19

   
**Rachik Sharma:** Now since it's an infant or a child, they do not have biometrics. So in MOSIP we have this concept of introducer and informant.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** We have this concept of introducer/informant which basically could be anyone.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** So in MOSIP there is no relationship that is maintained between the person who is reporting the birth and the child itself. For us it's always an introducer and we cannot collect the biometrics of the child because their features are not yet developed. So in this case we are simply trusting CRVS saying that whatever de-duplication and all you have done we are okay with it.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** This note here is to be modified saying here we have given that infants are individuals between 0 to 5. But this is not a definite rule. The demarcation between a child and an adult can be driven from country policy.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** It could be up to seven years, 10 years, whatever they want.  
   
 

### 00:08:24

   
**Rachik Sharma:** Second, when it comes to real use case, if somebody after a certain age has not updated their biometrics once they have become eligible, they'll still fall under this category because their biometrics are not associated.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** Because once a person's biometrics are linked with the ID then the expectation is all the changes or updates they want to do — they do it using biometric authentication. So slight change will come in.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** Okay. Now this is a simple step-by-step process. Once you go through it you will get to know — it's just getting the details, sending the request to MOSIP, then MOSIP creates packet using the create packet manager API, then you trigger the  
**Keshav Singh:** Amen.  
**Rachik Sharma:** workflow. Workflow trigger is nothing but to start the packet processing. Then it goes through different stages — quality classifier, demographic de-duplication, basic packet structure check whether there is no issue with the way the packet is created.  
   
 

### 00:09:33

   
**Rachik Sharma:** Once it is done, we create an ID and we — now here we have written that CRVS has the capability to listen to this WebSub topic where once the ID is created they can get the ID and if they want they can print it on the birth certificate or if they want for any other purpose or they want to link the birth registration number with the national ID they can also do that. So up till now this was something that we have written here but in today's discussion  
**Keshav Singh:** Excuse  
**Rachik Sharma:** it has come that this is not a default setting for CRVS–MOSIP integration. Once the details are sent to MOSIP, MOSIP will simply create the ID and the  
**Keshav Singh:** me.  
**Rachik Sharma:** notification will go to the resident directly — in this case it could be parent. Since when we were implementing this there was a need in the country which is Tonga that they wanted to have this update given back to the CRVS with the credentials. We thought that we will make it as a default functionality but in discussion today it is clearly called out this is not a default functionality and should not be part of the default documentation.  
   
 

### 00:10:39

   
**Rachik Sharma:** It should be a separate section. If you want to get the credential back once MOSIP has done — there is a way to do it but it's not by default.  
**Keshav Singh:** Got it.  
**Rachik Sharma:** Okay then another scenario is if duplicate request comes in.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** Okay, there are different scenarios for RID and all. Once you go through the document, you'll have all these questions.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** What is RID and all? Okay, I'll explain them now also. But in general, go through it. The idea is since de-duplication check is done by CRVS, for these scenarios,  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** we are not doing any de-duplication within MOSIP. So in case somehow a duplicate request comes in, either MOSIP will override the existing data or it will create a new ID. So there could be a scenario if CRVS has sent two different requests with the  
**Keshav Singh:** Okay.  
**Rachik Sharma:** same details of a child.  
   
 

### 00:11:41

   
**Rachik Sharma:** The child can have two also. That's a kind of an integration limitation.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** And it is listed here also. If you go down integration limitations — duplicate request rejection. Since CRVS is the source of truth,  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** MOSIP currently does not reject duplicate birth requests received from the CRVS system. This can result in multiple UINs for the same infant or an update of a death flag for the same deceased. De-duplication is expected to be handled by CRVS. So this is where the same point comes in that we are considering them as a source of truth — specifically for death and birth.  
**Keshav Singh:** Got it?  
**Rachik Sharma:** Okay.  
**Keshav Singh:** Because biometric is not there. This problem  
**Rachik Sharma:** Because biometric is not there for a child.  
**Keshav Singh:** is  
**Rachik Sharma:** Or for the deceased also. And that is also a reason. Another reason is CRVS is considered to be a legal authority in a country for all the vital events in a person's life.  
   
 

### 00:12:47

   
**Rachik Sharma:** Okay. So ideally what happens is when we said that earlier our stand was that if the duplicate  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** request comes in we'll add some validation from MOSIP's side and packet can be rejected but their point of view was we are doing our due diligence, we are doing everything — why MOSIP needs to do the same checks again? If we are doing it, we are the source of truth — you simply accept the request. So  
**Keshav Singh:** Mhm. Mhm.  
**Rachik Sharma:** they were rigid about this thing that they have to be the source of truth.  
**Keshav Singh:** Uh so now the limitation occurs because of that only right — in case of child in case of  
**Rachik Sharma:** limitation is —  
**Keshav Singh:** child  
**Rachik Sharma:** So that's one of the reasons for limitation because we are not doing any de-duplication from our side and we cannot also because it's a new record in the system and also since there are no biometrics the only de-duplication we can do is demographic data.  
   
 

### 00:13:46

   
**Rachik Sharma:** Now demographic data could be similar for two children. There is no way to actually verify because we are not collecting documents also.  
**Keshav Singh:** can be. Okay,  
**Rachik Sharma:** Huh?  
**Keshav Singh:** it can be  
**Rachik Sharma:** So even if our de-duplication process gives out three or four results against a particular request saying that the name and the gender and the DOB is same there is no way to actually verify it. And in case of Tonga since the population was very less we did not go into that direction also because it's a small country and there is no manual verification stage there — that's how they wanted it. So multiple reasons but point is there is no de-duplication as of now. Now for failure  
**Keshav Singh:** Okay.  
**Rachik Sharma:** handling — if anything is failing within MOSIP, we are not sending any notification to CRVS.  
**Keshav Singh:** All right.  
**Rachik Sharma:** We will only send notification to the resident directly. The reason for that is — one reason is it's a foundational ID.  
   
 

### 00:14:49

   
**Keshav Singh:** Okay.  
**Rachik Sharma:** CRVS has no purpose of knowing whether the packet has failed or not. If there is any technical failure within MOSIP, we have retries, reprocessing functionality. So there is already someone who is sitting on the MOSIP side and if there are technical failures we do have the provisions to reprocess the packet until and unless it is rejected or approved. So if it's under processing no need to send notification; once it's approved or failed you send  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** notification to citizens directly because even if you send notification to CRVS, CRVS actually cannot do anything — the person will have to come again. So person has to be notified saying that whatever request you submitted is rejected. You have to come again and submit a new  
**Keshav Singh:** And is there any UI for this?  
**Rachik Sharma:** request.  
**Keshav Singh:** Rajik — like if once any data for adults — for that matter you say — if data has been shared by  
   
 

### 00:15:59

   
**Rachik Sharma:** Mhm.  
**Keshav Singh:** CRVS that is updated through packet manager through API and gets updated within MOSIP — right if there is any change that has to be made —  
**Rachik Sharma:** Huh?  
**Keshav Singh:** to this adult's data on request or anything. So do they have any UI to do that also?  
**Rachik Sharma:** Mhm. Mhm. The only UI that will come in picture for any such record update is Resident Portal UI.  
**Keshav Singh:** No.  
**Rachik Sharma:** Okay.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** In this case and also — do not consider adult in this scenario. There is no adult scenario as of now. I'll explain in 2 minutes. So I'll come back to this. I'll finish this. So basically if something is getting rejected here we had written we will publish a failure event to WebSub topic  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** detailing the rejection reason then CRVS will subscribe to that topic and get to know why the packet failed.  
   
 

### 00:17:00

   
**Rachik Sharma:** All of these things will not be there by default.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** We will simply update the resident saying that your request is rejected. You come again and register — which is a very small percentage of packets that will get rejected because we are not doing any validation on our side. Technically it can only be something like Sashi was saying — okay it goes to ABIS or it goes to some stage and it never comes out of that stage due to some technical failure. Then for that the SI who is responsible for running the MOSIP system in the country will do their due diligence and make sure that the system is working correctly.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** And also there is no uniqueness conflict that can be there in the validation because we are not doing de-duplication. Mismatch can be there but it's not a valid scenario because schema is one of the prerequisites before you start this integration.  
   
 

### 00:18:03

   
**Rachik Sharma:** The same schema that MOSIP is using has to be used for creating the packet in this case. So points are also not valid. Missing or invalid data — this could be a valid point but again it depends.  
**Keshav Singh:** Thank you Jesus.  
**Rachik Sharma:** Okay. So we can keep this but these two are not valid. So in general what we have to say in failure handling is — technical failure is the main point — MOSIP has the capability to retry.  
**Keshav Singh:** That's funny.  
**Rachik Sharma:** We have given it here also. Retry mechanism automatically attempts to reprocess the failed request. Until and unless you have reached a stage of approved or failed — once it has reached the stage where it says packet approved UIN created or packet  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** rejected UIN not created — until then no notification to citizen also. Now similarly for death — for death we introduced four new fields in the ID schema.  
   
 

### 00:19:06

   
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Out of four we are only using these two as of now — declared as deceased (boolean flag) and deceased declaration date (on which date the death request is coming in). Everything remains same. Here we will get deceased information. Again the person who is reporting the death could be anyone. For us it's informant.  
**Keshav Singh:** okay.  
**Rachik Sharma:** We will validate both of their UINs and one more field  
**Keshav Singh:** Okay.  
**Rachik Sharma:** that has to be sent in the request is this eSignet user info token. This is applicable for both birth registration and death also. eSignet user info token is nothing but — if I am a parent registering for birth,  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** I will authenticate against eSignet and then eSignet returns a user info token. That user info token has to be passed in the request as one of the fields in ID schema.  
   
 

### 00:20:02

   
**Rachik Sharma:** Similarly for death — whosoever is doing the death registration and CRVS sending us the request — the person who is authenticated using the ID against eSignet — his user info token will be here. This is just an extra step where validation will come in from MOSIP side to say that the person who has authenticated is valid.  
**Keshav Singh:** This is for informant, right?  
**Rachik Sharma:** This is informant in both the cases.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** Okay. Then in death also they send us the packet.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** We trigger it. If everything goes fine the flag will be updated to yes — death declaration flag — and UIN will not be deactivated.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Now why UIN is not deactivated? That story has also to come here. Okay. So now I'll stop at death. Now I'll say what is required to be updated here as a story part for both birth and death.  
   
 

### 00:21:06

   
**Rachik Sharma:** For birth we have to clearly call out here saying that biometrics are not associated for birth because a child cannot have biometrics. That's why we allow all the packets from CRVS. Whatever packets are authorized by CRVS we allow them to come into MOSIP and we allow them to create the  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** UIN. We do not share any notification back to them by default. If you want to have this then it's a separate section that we can link here. Third thing is for death registration also — why we are not deactivating the UIN automatically is because once a person is deceased there are multiple services which are still associated with the ID of the person. The family or the close ones of the family who want to access the  
**Keshav Singh:** Okay.  
**Rachik Sharma:** services of the deceased UIN will still require the UIN to be active. So from foundational ID system point of view — since it's an interconnected system and it is being used by  
   
 

### 00:22:04

   
**Keshav Singh:** Okay.  
**Rachik Sharma:** downstream services left right center — we cannot automatically deactivate it. So that is the point.  
**Keshav Singh:** Example could be bank thing. Example could be like insurance.  
**Rachik Sharma:** Insurance is one major example.  
**Keshav Singh:** Insurance. Yes.  
**Rachik Sharma:** Then bank is one example. Then there could be a scenario where you want to — let's say you use your ID to pay monthly bills somewhere. Now you want to change that ID.  
**Keshav Singh:** Now — as in case of Indian land registry systems — certain states have started using Aadhaar — so likely  
**Rachik Sharma:** Now let's say if a land has to be transferred and you have the mobile of the deceased — you get the OTP there — you authenticate and then you put in a request to change the ID. First you prove that it's the ID of the person with me and then you submit a request for ID change and it can take longer than expected to change the ID.  
   
 

### 00:23:15

   
**Rachik Sharma:** Okay, but until then you should be able to claim your right to the land.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So for all these various purposes this has to come out as a story why we are not deactivating UIN directly for deceased persons. Now third scenario comes in — demographic data update.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Now there are two scenarios here. One is — it could be a demographic data update for an infant. Let's say country has decided that first five years (0 to 5) is the age which we consider any request to be child — above that we expect them to be adult, meaning biometrics have to be there. So first thing — if any request comes above that age — that validation we have — because in MOSIP any packet that comes in — we pass them through this  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** child filter where we validate whether the age of the packet based on the date of birth given in the packet is greater than the configured age or less than the configured age. If less, then we allow the packet.  
   
 

### 00:24:20

   
**Rachik Sharma:** If it's more —  
**Keshav Singh:** And that age is configurable,  
**Rachik Sharma:** configurable — by default I think it's 5 years in MOSIP but it's up to the  
**Keshav Singh:** right?  
**Rachik Sharma:** country. So currently also in the integration there is no support for new adult birth registration. Let's say today in the country the integration starts and I go to the CRVS system saying that I'm 27 years old or I'm 32 years old or 35 years old and now I want to register for a birth registration. They will not send us the request for UIN creation because we will reject it because it's an adult. If you are an adult, you need to give biometrics to register yourself. So that logic is already there.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So there is no new adult birth registration as of now.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Similarly for demographic data update — one scenario is infant or pre-biometric stage. So the story has to come out here that any person who is under a certain age as per the country policy and biometrics cannot be  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** collected — we consider them as infant. But it does not limit to that — if they do not have a biometric linked to the ID — that's the second point — then also we can consider that as a demographic data update for child although they are not child because they do not have a biometric associated. So if they do not have a biometric associated — which in larger scenarios will  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** always be an infant — but in some scenarios it could be someone who is not an infant but let's say a minor 7 or 8 years old and they do not have biometrics — in that case also whatever request CRVS sends — we will take that request based on the correction that they want to do. Correction is not limited to — but suggestion is to have name, DOB and gender. So this list is not exhaustive.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** This list is just suggestive. I think we have a note somewhere here.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Okay. The list is suggestive.  
   
 

### 00:26:21

   
**Rachik Sharma:** Here — supported demographic fields for update — we say name, DOB or gender. This is suggestive, not restrictive. Countries can configure any demographic attribute. So if a country decides — okay from CRVS we should allow the address update — and they want to add that field in the update — we are okay with it.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** It's up to them — but it should be an ID which does not have biometrics linked. Or if country wants to decide using the age then it should be an infant. Then only this update will take place. Second thing — IDs which have biometric linkages — it could be adults or individuals enrolled with biometrics into the ID system. For these we are not doing any demographic update. For these user will have to go to the MOSIP registration center for updates — basically ID system. Because once you have your biometrics linked it is the most assured  
**Keshav Singh:** Mhm.  
   
 

### 00:27:27

   
**Rachik Sharma:** way of proving that you are the one who wants to — oh one second.  
**Keshav Singh:** Is it washroom? Come. Uh, please. Yes, I see.  
**Rachik Sharma:** One second. Keshav.  
**Keshav Singh:** Hello.  
**Rachik Sharma:** Okay. So what I was trying to say was — biometrics is the only way to actually assure that you are the person because there is no better way than biometrics and there is no way to send the biometrics for CRVS — that kind of design is not in place. So if it's an adult and biometrics are linked with it — there is no point in sending request to MOSIP for any update. One reason is this — it has the highest assurance level that we can achieve for any update. Second reason is again the same — the person who is going to the CRVS thinking that  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** they'll do a name change from CRVS side — does not understand the impact that once a name is changed in the national ID also — they might start facing issues.  
   
 

### 00:28:45

   
**Rachik Sharma:** Let's say in passport they have a different name. Now in ID their name is different. They'll start facing that issue when they start accessing the services. So the implications might not be known to the citizen itself — that updating a name change from  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** CRVS might result in many different issues.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So if you really want to do any name change or gender change or DOB change or any other address update change, you should go through a proper biometric process within MOSIP ID system where you can be sure that whatever changes you are making will not result in interrupted services. So that's why there is no point in getting a request from CRVS for adults.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Okay. And it's not —  
**Keshav Singh:** for demographic change — especially in case of name change — so what are we telling to CRVS — are we accepting any such request or not  
**Rachik Sharma:** No — we are not. We want to associate —  
   
 

### 00:29:50

   
**Keshav Singh:** okay  
**Rachik Sharma:** so that's where the documentation overall will come in — where for adult demographic data update — which is this point, point number two — okay, for this point — okay — this is for child  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** and then this point 3.3 — there has to be a story to rationalize and put out the risk — so there could be a risk section  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** here — to rationalize — I've already given but yeah we'll have to make it in a section saying that if you allow such kind of updates for adults from CRVS — these are the risks that you can run into. One risk — it's not the best way to authenticate an individual who already has biometrics linked. Second — multiple downstream services can be affected — essential services — because of this demographic data change.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Okay.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Now citizens themselves — and second thing is — foundational ID life cycle or ID update is actually the part of ID department not the CRVS department.  
   
 

### 00:31:05

   
**Rachik Sharma:** Name change is not a kind of a vital event that CRVS system should be worried about.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** In cases of child,  
**Keshav Singh:** Yes.  
**Rachik Sharma:** we are okay because they are the most authentic source of truth because they are the ones who are actually collecting the documents. We do not do that. But in case of adult — if biometric is associated — that means that person now is part of ID system properly.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Now to manage the ID should be done from ID system — not from the CRVS system — because technically it's not part of their remit.  
**Keshav Singh:** Got it.  
**Rachik Sharma:** So it has not to be put in this way that CRVS has no legal authority. They have legal authority — it's fine — but it makes more sense and it is more secure if a person's ID is managed from MOSIP (national ID) department — biometric re-authentication can happen if they are submitting a document for — let's say — name  
**Keshav Singh:** Um,  
   
 

### 00:32:00

   
**Rachik Sharma:** change. Currently we are not getting any document from CRVS. They can show that document in the national ID department — okay  
**Keshav Singh:** right.  
**Rachik Sharma:** see — here is an approval of my name change — now I want to change my name — so the same can be notified to multiple downstream services. So there has to be a risk-associated  
**Keshav Singh:** Means what I understood —  
**Rachik Sharma:** section. Hello  
**Keshav Singh:** a national ID system has rules defined and ideal practices which should be followed and a country implementing national ID system like MOSIP or any other — and the citizen of that country should follow — even if there are name changes and there are different practices of name change — passport or other services — PAN or something. So that should be — to avoid that — national ID system should not accept — or MOSIP should not accept — demographic change from CRVS and it is not advised —  
**Rachik Sharma:** for the adults or for the people who have biometrics linked in that  
   
 

### 00:33:26

   
**Keshav Singh:** yes  
**Rachik Sharma:** and the point is — it has to go as a recommendation. So it's not a hard and fast rule. When somebody reads it, it should clearly say — if you do this, these are the risks you can encounter — which  
**Keshav Singh:** Mhm. Like in India also — taking example from the best national ID system implementation — here also when PAN card — there was a talk of linking the PAN card — there were people raising concerns — whenever there is a request that you should link anything to Aadhaar — there is a concern raised  
**Rachik Sharma:** Huh? Huh?  
**Keshav Singh:** because what is advised is — you have different identities for entirely different purposes. PAN card has a different purpose, passport has a different purpose. So if the request is even coming from  
   
 

### 00:34:37

   
**Rachik Sharma:** Yes.  
**Keshav Singh:** CRVS to update the demographic data in national ID system — it is a risk. First thing. Second thing, unless the country has linked every other services or ID thing with national ID — which is again a huge task for country and decision making of the country — whether they will go for it or not — in that case also there are chances that no country will link all the services to national ID again.  
**Rachik Sharma:** Understanding-wise it's correct. But that we do not want to put here.  
**Keshav Singh:** Huh.  
**Rachik Sharma:** What we want  
**Keshav Singh:** Understanding-wise it is correct.  
**Rachik Sharma:** to  
**Keshav Singh:** But just to articulate we have to articulate different points here which should be valid in case of this particular document  
**Rachik Sharma:** Huh.  
**Keshav Singh:** and integration point of  
**Rachik Sharma:** Huh?  
**Keshav Singh:** view.  
**Rachik Sharma:** So here — the idea is — first thing — it's a recommendation that any country should not allow adults or IDs with biometrics already  
   
 

### 00:35:55

   
**Keshav Singh:** Mhm.  
**Rachik Sharma:** — should not allow demographic updates directly from CRVS. This is — in one line — the recommendation. If you do this, these are the list of risks that you can encounter. After that, there can be a note saying — even if the country wants to do it,  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** it's only a configuration and country is free to do it — if they deem it necessary that it has to be done from CRVS system.  
**Keshav Singh:** In that case also — should we not recommend that it should be national ID? If you have implemented national ID then if you want to update the demographic data you should be updating it through the national ID system or foundational ID system — not the  
**Rachik Sharma:** That's the recommendation — you should not allow from CRVS — but instead citizen should  
**Keshav Singh:** registry.  
**Rachik Sharma:** visit MOSIP ID system. This is the recommendation. Why it is a recommendation will be explained in the risks that can be associated with direct updates from CRVS for such cases.  
   
 

### 00:37:15

   
**Rachik Sharma:** Even after that — if somebody wants to go and say — I have covered all the corners and in my country this will not be an issue — I want to do it from CRVS — then it's their decision at the end. It's not an integration limitation — it's just that by default it will not be there in integration. If you want to do it — you take the code — customize according to your needs — and you do  
**Keshav Singh:** practices recommendation. And it is what we are giving. It is not the tech limitation  
**Rachik Sharma:** No, it's not a tech limitation.  
**Keshav Singh:** platform.  
**Rachik Sharma:** It's a MOSIP recommendation — what we feel necessary that countries should know before making any such decision.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So that has to come here for adult demographic data update — basically saying that demographic update for IDs with biometric linkages. And all these — whatever I've written — has to be elaborated — saying that this could be a scenario where you want to do this but we do not recommend it — do not do it directly from CRVS — visit MOSIP ID — why you should visit — refer below the risks associated with the direct demographic update for an adult and why we suggest national ID — then list down the risks — then say even after  
   
 

### 00:38:34

   
**Rachik Sharma:** that — if a country decides that they want to go ahead with this — this particular feature is not part of the default integration but there is no tech limitation — you can take the code, customize according to your needs and go ahead with the adult updates as well. Similarly — based on  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** whatever changes we'll do in the above section — integration limitations — the headings will be changed — something like limitations: de-duplication check is a limitation — we are not doing it. Packet status of rejected or update is not given to CRVS — it's a limitation — or we can say not supported or features not supported — rather than limitation. Similarly for offline integration — there is no scenario for offline integration — once you have collected the data. One thing for offline also — we should have a separate section which has come recently — although it is written here that integration only works when online connectivity is available — as eSignet authentication is a necessary step before a request is submitted by CRVS — offline/no connectivity support is under development — we had said — but this has to be removed — there is nothing under development for offline.  
   
 

### 00:39:58

   
**Rachik Sharma:** The scenario — why this has to come in the picture — is CRVS was saying that there are scenarios where they go to remote location — villages — and the field agent who is going there collecting the data — and at that point it is not possible to get the authentication of the parent or informant who is the details of the child for birth registration. Now for such scenarios — we should be able to send the request directly or we have some offline mechanism to do that. Our recommendation here — or our stand — is — no matter that you are in a remote location — let's say — and you have collected 100 records —  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** hello  
**Keshav Singh:** Yes.  
**Rachik Sharma:** Now you have gone to a village and collected 100 records for children with name and UIN of the parents. Now once you are going to any CRVS center where online connectivity is there — because to send a request you need online connectivity — because you are using an API —  
   
 

### 00:41:23

   
**Rachik Sharma:** In that case since you are not able to authenticate parents — the registrar/admin person who is sitting at the CRVS center and approving the request —  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** all these 100 records can have the eSignet authentication of that particular person — because we consider that he's a legal entity sitting there. What we need is the eSignet user info token of the person who is approving the  
**Keshav Singh:** Okay.  
**Rachik Sharma:** request in case you are not able to authenticate parents. So that's why there is no support for offline integration. Even if you are collecting data offline, you come online, you authenticate with the ID of yourself — or the ID — basically the ID of the person who is approving  
**Keshav Singh:** Yes.  
**Rachik Sharma:** the request. It could be a registrar. It could be a CRVS agent who is legally assigned to that role of approving the request and sending it to MOSIP. But we need the eSignet user info token — because we want to know from where the request came or who approved these requests. Because tomorrow — let's say — 1,000 or  
   
 

### 00:42:25

   
**Keshav Singh:** Okay.  
**Rachik Sharma:** 10,000 records for birth — let's say they were illegitimate — somebody was just registering children to get UINs and get the services in a country — and audit comes in — MOSIP should be able to say — okay — see this is the person who approved all these requests. So no offline connectivity support. This is the logic — for MOSIP's audit and MOSIP's purpose — we just need the ID or user info token of the person who is approving the request in case the people are coming to a center where there is online connectivity. Well and good — you take the authentication — you do the authentication of the informant/parent and send us the eSignet user info token. In case you are collecting data offline and then you are going to a center where there is connectivity, you ask the registrar to do it.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** So that's one thing.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So overall as of now — this is the scope written here and it has to be modified in a storytelling way where we justify each action — why we are recommending something — and note that nothing is a technical limitation.  
   
 

### 00:43:40

   
**Rachik Sharma:** Apart from some steps like de-duplication, we are not doing offline authentication — there is no way to do offline authentication. Other than these two-three points — most of the things are recommendations that MOSIP gives. Similarly — this page will be overhauled — and the structure also — like if you see here — the way it is written, it's kind of confusing now — what I feel — because new requirements are coming in and some points might not address or  
**Keshav Singh:** Thank you. To that I'll have some suggestion, Rachik — but before that  
**Rachik Sharma:** uh  
**Keshav Singh:** do we have anything else — like approach is the next topic?  
**Rachik Sharma:** So next is approach. Now how we are enabling them to do this?  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Very clearly it gives us the prerequisite — that at a high level — it is clear that we have opened an API from packet manager — so that you can directly hit that API and create a packet — instead of going through registration client — so basically you become an online registration client for us.  
   
 

### 00:44:51

   
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So that packet can be created and processed. To do that — what is required — is clearly written here — that you need to create a new client, assign a role,  
**Keshav Singh:** H  
**Rachik Sharma:** obtain the token for API — because this is the API call that you will make.  
**Keshav Singh:** I lost you.  
**Rachik Sharma:** Hello.  
**Keshav Singh:** Hello.  
**Rachik Sharma:** Hello. Hello.  
**Keshav Singh:** Yes. Yes. Now I can hear you  
**Rachik Sharma:** Okay. So here the list of steps is very clearly written — what is required to be done even before you submit a request to MOSIP. Similarly — it's very similar to what we have in registration client. So if somebody's setting up a MOSIP system in a country,  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** these are the steps that they'll do. They'll create a role, they'll set up a registration center, they'll request a machine, they'll register an operator profile, they'll map that center to the machine.  
   
 

### 00:45:48

   
**Rachik Sharma:** Then every ID — that is every request that is generated for UIN creation — will have an application ID.  
**Keshav Singh:** All  
**Rachik Sharma:** Okay. So, same thing CRVS will have to do because now they are becoming a kind of online registration client and this work falls in their kitty or SI's kitty — whosoever is implementing it. Now once everything is done — you send the request to the create packet API and you submit data using create packet  
**Keshav Singh:** right.  
**Rachik Sharma:** API, upload the packet to MOSIP, then trigger processing, validate, then generate identity credential. Then these seven or eight points will not be here because these two points are exclusively talking about a WebSub event that will be published so that CRVS can get the update. This is not part of default. If you really want it — this will become a separate section.  
**Keshav Singh:** So was it a point of conflict that happened recently? They were demanding something like this to be default or  
**Rachik Sharma:** Huh. So earlier when we did this release — at that point — all of us were under the apprehension that everyone is agreeing to providing updates to CRVS. So at that time — since Tonga was a country — we asked the SI to subscribe to a particular WebSub topic and as of now they only get the credentials for the packets that are approved and UIN is created. Then they came back saying — what about the packets which are failing? We also want to get the updates for the packet that are failing — because then we'll get the info and then we can resubmit the request with corrections. So that's why we were thinking that this will become part of default — but it is now clarified that this will not be part of the default because CRVS has nothing to do with the identity credentials. Their only purpose is to submit the info to us — and then what we do on MOSIP side is between us and the resident.  
**Keshav Singh:** Yeah.  
**Rachik Sharma:** If  
   
 

### 00:48:01

   
**Keshav Singh:** And if for that matter — if they have done the due diligence and they  
**Rachik Sharma:** if  
**Keshav Singh:** have created the records and then the packet manager starts — means their API —  
**Rachik Sharma:** Mhm.  
**Keshav Singh:** their record hits the packet manager APIs and that flow is initiated. So no point worrying about it even post that — in case of failure only.  
**Rachik Sharma:** uh it's not related.  
**Keshav Singh:** Yes.  
**Rachik Sharma:** In case of failure also — let's say CRVS has registered a request —  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** now they have created a birth certificate and they have given it to the citizen. Now request comes to MOSIP and it is stuck in MOSIP — let's say for 5 days — due to some technical issue — and at the end it gets failed. Now what will CRVS do in this?  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Now what citizen has to do is — they have to resubmit the request for UIN — right? So that's why CRVS should not be concerned about the update of a particular packet — because there is no dependency on that. They do not have any dependency. But even if they want to do it — then there is a way to do it — but it should not be part of default integration. This has to very clearly come out in a separate section —  
   
 

### 00:49:17

   
**Rachik Sharma:** Why we do not want to send notifications to the CRVS?  
**Keshav Singh:** Yes.  
**Rachik Sharma:** Why we think that from foundational ID perspective it is not necessary for CRVS to be updated on this — because first — there is no dependency. Second — there is nothing CRVS can do to resubmit the request — citizen has to be informed to resubmit the request — and that notification we send to the citizen — saying that there was a request for a new ID creation and it is now failed — you have to come again to the center and resubmit the request. And third — the percentage of such packets getting failed and UIN not generated is very less because MOSIP already has a way of internally reprocessing the packets if there is a technical failure. Fourth point — there is no data validation actually that MOSIP is doing.  
**Keshav Singh:** I'm moving  
**Rachik Sharma:** So most of the scenarios will always be technical failures within MOSIP for which the  
**Keshav Singh:** this.  
**Rachik Sharma:** national ID department is responsible to make sure that all the packets go through reprocessing and they reach the end stage of approved or rejected.  
   
 

### 00:50:26

   
**Rachik Sharma:** Okay.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So these three or four points we can list down — why we do not want to send the notification to the CRVS system — specifically — but instead to the  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** citizen. So these two points will be removed from here and there should be another section for notification — saying to whom it will be sent — why it will not be sent to CRVS — what is our recommendation — and why it is not supported by default.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Even after that — if somebody decides — no I want CRVS to be updated — then there is a way to do it  
**Keshav Singh:** invitation.  
**Rachik Sharma:** — using this WebSub topic. Okay. So that's one thing that has to be changed here. Then coming to technical details — here — in technical details — it's similar — basically whatever I have written in approach — I have expanded and elaborated it here — how do you create  
**Keshav Singh:** Mhm.  
   
 

### 00:51:25

   
**Rachik Sharma:** a CRVS role.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** This — I don't think we need to change — because standard process — all of this will be same. Only thing that I want to explain for your understanding is step eight — which is create API — which is not coming out here  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** very specifically. So whether we want to put it in the approach or in technical details — I think it should be in the approach — because one thing that actually determines which Camel route — or basically which stages in the registration process a packet will pass through — totally depends on this `process` and `source`. We do not have separate APIs for different flows — we do not have another API for birth and another API for death and another API for update. Using the same API request structure — based on the value that is given in the `process`.  
**Keshav Singh:** Huh? So this API documentation should be improved — right?  
   
 

### 00:52:31

   
**Rachik Sharma:** Uh-huh.  
**Keshav Singh:** Make it conditional — explaining in the very beginning these two things — `process` and `source` — and then explaining all other parts of this API — here we have just put the API  
**Rachik Sharma:** API document is here — we have put the  
**Keshav Singh:** sample  
**Rachik Sharma:** request — it's a sample request — we  
**Keshav Singh:** request — and have we explained that  
**Rachik Sharma:** — what are the different fields that are given here. So the whole point I think that should go in approach is — whether a packet is considered as a birth  
**Keshav Singh:** Okay.  
**Rachik Sharma:** registration packet or death registration packet — or in future an update packet — all of this is decided by `process` and `source` combination.  
**Keshav Singh:** So it can have different value — right.  
**Rachik Sharma:** `source` will always have the same value because it's coming from  
**Keshav Singh:** Okay.  
**Rachik Sharma:** single CRVS system.  
   
 

### 00:53:37

   
**Keshav Singh:** Right.  
**Rachik Sharma:** `process` will be different.  
**Keshav Singh:** And  
**Rachik Sharma:** So for birth it will be `CRVS_NEW`.  
**Keshav Singh:** process.  
**Rachik Sharma:** For death it will be `CRVS_DEATH`. For update it will most probably be `CRVS_UPDATE`. So based on the `process` and `source` we decide how the packet is treated and which stages it goes through. So CRVS do not have to be concerned about different set of APIs for each different flow — it's a single API. That thing is not coming out very clearly. I think we can add this in approach.  
**Keshav Singh:** Which means — if there would be separate requests — in ideal case of MOSIP and registration client — different APIs to do those things — here we have single API in which there are two conditions listed above and the `process` and its value will define how it will be processed by the packet manager.  
**Rachik Sharma:** by the registration processor — and how MOSIP  
   
 

### 00:54:39

   
**Keshav Singh:** Right? Registration processor. Okay.  
**Rachik Sharma:** will identify whether it's for birth or death or any other workflow. So in general — this `process` value is very important. Whatever `process` value you give — packet will be created as per that and processed as per that. Then there are different fields which are already given — like we have given — when initiating an infant birth request — when initiating a death registration request. Now tomorrow — if we start supporting update — then it becomes `CRVS_UPDATE` and we'll say — when initiating update request for infants specifically. So that has to come out clearly. I think approach is the right way — because here if somebody comes and sees — they'll think there are only two supported processes. In the approach we can list down the supported processes — based on the tech design — we can have a discussion — but this has to be updated. So this is the last page — technical details.  
   
 

### 00:55:47

   
**Rachik Sharma:** Okay. Now adding to the scope.  
**Keshav Singh:** Got it.  
**Rachik Sharma:** Things that will get added to the scope are these. This is the next part we discussed recently — rare scenarios. There are three rare scenarios. One is — let's say CRVS has submitted a request and five days later — one month later — they realize that the document that was given — somebody comes and tells them that this birth that was registered is an incorrect birth — or all the births that are having the birth document or birth record for this hospital are fraud — and CRVS decides to send us requests saying that  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So by mistake there was a fraud — or it can be incorrect. Fraudulent is not the right term — fraud is one of the scenarios — but at the end CRVS realizes that whatever birth request they have given — or  
**Keshav Singh:** Mhm.  
   
 

### 00:56:50

   
**Rachik Sharma:** whatever birth certificate they have assigned is not a valid one due to some data discrepancy or other misinformation and they want to nullify that  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** record from their side. They can nullify the birth certificate and ask to create a new birth certificate with the correct details. But they want us also to deactivate the ID.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Similarly for reactivating an ID — which is an illogical scenario — but they came up with it. I mean — once you have deactivated why would you want to reactivate? And this is coming from  
**Keshav Singh:** Thanks.  
**Rachik Sharma:** the point of view of birth registration — that it was a fraudulent birth — we deactivated — later somebody came with a court order saying it was a valid birth — now reactivate the ID. It doesn't make sense — we have decided how we will deal with it — but that's what CRVS was expecting. And third is reversal of death flag — basically somebody was mistakenly entered as deceased.  
   
 

### 00:58:04

   
**Rachik Sharma:** Now one year later — he was in a war-torn country — on the battlefront. Six months later or one year later he comes back home and he gets to know that he is marked as deceased. He goes to CRVS saying — see I am alive. I am the person. Please nullify my death certificate and ID system should also reflect the same.  
**Keshav Singh:** In fact, for fun — pun intended — I think these are all valid scenarios. Even reactivating — one example I was reading on social media —  
**Rachik Sharma:** Mhm.  
**Keshav Singh:** like somebody conspired against somebody to get his insurance money — documents and verifications — death certificate — he was declared dead — and now he's fighting to establish alive again, right?  
**Rachik Sharma:** So that is a valid scenario. But that will not be a valid scenario for  
   
 

### 00:59:13

   
**Keshav Singh:** So,  
**Rachik Sharma:** MOSIP — because we actually do not deactivate any ID for adults. These two are specifically for child birth scenarios.  
**Keshav Singh:** okay.  
**Rachik Sharma:** So from birth scenario perspective — if you already deactivated something — why would you want to reactivate it  
**Keshav Singh:** It says yes.  
**Rachik Sharma:** again?  
**Keshav Singh:** Correct. Correct.  
**Rachik Sharma:** That means that in first place your deactivation was not  
**Keshav Singh:** So when they come up with this kind of request do they provide the reason for it  
**Rachik Sharma:** proper  
**Keshav Singh:** like scenario for it — in what kind of scenario it is valid — do they provide?  
**Rachik Sharma:** I'll come to that. So these are the three different scenarios that have to be added to the scope now.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** Now, first thing is — this one — incorrect birth. The word should be incorrect birth or mistaken birth or something like that. Now what we are proposing — since we want to — the reason — first thing is why we are supporting this.  
   
 

### 01:00:21

   
**Rachik Sharma:** We are supporting this because again this is a child birth scenario. CRVS is doing all the document verification from their side. If they are finding something — that means it's legitimate information that they have found and it should be passed on to foundational ID system.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** That's why we are considering this scenario. Now when they are sending us the request — since there are no biometrics involved — we allow them to send the request and we allow them to raise a request for deactivation — but we'll not do it directly. What we will do is — similarly to deceased flag — we'll add a new flag saying `deactivate_id` that will be part of ID schema — which they'll mark as true.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** Then a new process will be defined which will clearly mark this packet as for deactivating a child ID. Then we will ask them a reason — it can be a string. They can give any reason.  
   
 

### 01:01:20

   
**Keshav Singh:** Mhm.  
**Rachik Sharma:** It can be — this is due to fake birth document from the hospital — the child is actually not born — it's a fraud. Anything they can pass — we take that. Any optional supporting document that they want to add — any court order or the fake medical certificate or hospital certificate that was there.  
**Keshav Singh:** Will we store this document  
**Rachik Sharma:** If they are passing it —  
**Keshav Singh:** somewhere?  
**Rachik Sharma:** we will not store it — but we will keep it with the packet for manual  
**Keshav Singh:** Okay.  
**Rachik Sharma:** verifier — because this packet will ultimately go to manual verifier. Similarly, now this request has come with all the details along with the other details that I was showing you in the API request.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** Most probably the API request will be updated for this to include all these fields.  
**Keshav Singh:** The same API is  
**Rachik Sharma:** Mostly it will be the same API that will be updated to include all these fields.  
   
 

### 01:02:25

   
**Rachik Sharma:** So all these fields — if they are filled — then we validate — okay it's a deactivate packet — and `deactivate_id = true` — that means it's a child deactivate packet — and now it will be given to manual verification. MOSIP will not do anything automated here. We have a similar thing for death and birth also. We follow the same API request structure. Different flags are there — you update the respective flags. Now —  
**Keshav Singh:** Love  
**Rachik Sharma:** there is a window — it's country-decided.  
**Keshav Singh:** it.  
**Rachik Sharma:** We have to — when we write the story — give a reason why we are suggesting a period of 60 or 90 days. One reason — in this time period — it is possible that the ID created for child is not being used for many essential services.  
   
 

### 01:03:24

   
**Rachik Sharma:** Because child is just born. The documents in the hospital are fresh for referencing.  
**Keshav Singh:** Amen.  
**Rachik Sharma:** It also gives a window for early incorrect or fraud detection. So that if CRVS knows that this is a time window — or country decides that this is a time window — there will be due diligence to make sure that every birth request is revalidated in case somebody comes in. Population will also know that if they have to submit a request for deactivation — they have to come within a window. But it is a country decision.  
**Keshav Singh:** Hello.  
**Rachik Sharma:** It's a configurable window — no hard rule — it's up to the country. They can keep it as one year, 2 years, 5 years — whatever they feel relevant for their case.  
   
 

### 01:04:23

   
**Rachik Sharma:** Based on the date of initial registration and the current date we can decide whether it's within a window. If within a window — fine. If not within a window — still we will accept the packet. This window is just for the manual verifier who will verify this request — saying that — see this  
**Keshav Singh:** Okay.  
**Rachik Sharma:** request has come these many days later — just information. Now,  
**Keshav Singh:** Okay.  
**Rachik Sharma:** if within a window — the record is flagged — `deactivate_id = true`. Now de-duplication will happen and it will be directed directly to the manual verifier stage. And then manual verifier will see the reason, any supporting document — they will make their own decision. If they want — they can offline connect with CRVS or the hospital or the parent to verify why this is an illegitimate birth.  
   
 

### 01:05:32

   
**Rachik Sharma:** All those checks are now left to manual verifier. What MOSIP is doing is rerouting the packet to the manual verification stage. If any duplicate request comes for the same UIN — we will reject it — because we can check whether the flag is already true for that UIN after the last request or not. Basically duplicate requests will be rejected — and then manual verifier of the country will decide whether it's true or false — approved or rejected. If they approve — that means they are okay with deactivating. A logic will be added within MOSIP to automatically deactivate the UIN.  
**Keshav Singh:** H.  
**Rachik Sharma:** No manual step.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** Earlier we were thinking that we will leave it to the country officials to do it from back end — but what SI is saying is — deactivate it directly if they are approving it — because manual verifier is making a decision. So if they approve —  
**Keshav Singh:** If they are objecting  
   
 

### 01:06:37

   
**Rachik Sharma:** If they are objecting —  
**Keshav Singh:** then  
**Rachik Sharma:** they will reject it. So they'll reject the packet.  
**Keshav Singh:** okay  
**Rachik Sharma:** If they reject the packet — nothing happens. ID remains active — no flag is added.  
**Keshav Singh:** it is — mhm  
**Rachik Sharma:** Nothing is added. So this is — approve or rejected — you are done with the request. If after deactivation — which is the second scenario — another request for reactivation comes in. And also — when we are writing the story for this scenario — this will clearly say why we are allowing the deactivation — because it's a child record — we trust CRVS — and anyway we are giving the packet to the manual verifier. Manual verifier of the country will be an authority designated for this role — and we already have a stage in MOSIP for manual verification.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So considering all of this — the packet is actually getting deactivated after an approval from the national ID department.  
   
 

### 01:07:38

   
**Rachik Sharma:** So we are okay to deactivate — and it is specific to the incorrect birth requests. Now — say somebody comes to CRVS and says — the birth that you  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** nullified is actually a valid birth and we want the ID to be reactivated — which is irrational. Our approach — we will recommend — there is no support for reactivation.  
**Keshav Singh:** s***.  
**Rachik Sharma:** There is no concept of reactivation within MOSIP for a child. Because if a child ID is deactivated — now let's say somebody goes to court — there's a fight between two parents — and one of the parents — to extract revenge — has deactivated the ID of the child so that no child benefits are given — and the parent starts getting problems — they go to court — get the court order — go to CRVS — saying — see this was deactivated — reactivate.  
   
 

### 01:08:43

   
**Rachik Sharma:** From national ID perspective — it doesn't make sense — because the ID is already deactivated. What you do is — submit a new request and get a new ID.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** That's our recommendation. There is no concept of reactivation within MOSIP or foundational ID. From foundational ID perspective — reactivation of a deactivated child ID does not make sense. We'll have to write in better words — but basically — we have to say — there is no concept like this.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Because — we cannot simply open it up for people to toggle on and off — ID is a crucial thing — somebody  
**Keshav Singh:** Correct. Correct. Great.  
**Rachik Sharma:** cannot  
**Keshav Singh:** It is crucial and practices would not recommend. There must be some standard guiding the foundational ID system and its practices — there must be — isn't it, Rachik — so in those guidelines also  
   
 

### 01:09:42

   
**Rachik Sharma:** Yes. Yes. Yes.  
**Keshav Singh:** it would be that you cannot play it like a general application — toggling switch on/switch off.  
**Rachik Sharma:** Huh. So that's one of the reasons. Second reason — why do you want to reactivate the same ID? You can get a new ID — submit a new request and get a new ID. So that is for reactivation.  
**Keshav Singh:** Yes.  
**Rachik Sharma:** Similarly for death — in this case it is valid that if somebody wants to reverse the death flag.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** To reverse the death flag — somebody — let's say — it's the same scenario. Somebody who was fighting on the battlefront comes back after one year and gets to know that their family thought that they are dead and they have marked them as deceased. There is no concept of deactivation for adult.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So — since there is no concept of deactivation for adult — there is no concept of reactivation also.  
   
 

### 01:10:43

   
**Rachik Sharma:** So all we can do is update the flag. Flag update is supported here. It will follow the same process. The story remains the same.  
**Keshav Singh:** All  
**Rachik Sharma:** We do not want to deactivate because there are a lot of interconnected services. So there is only a flag that is added — so that service portals can know during authentication that the person is deceased. Similarly — since there is no deactivation — there is no concept of reactivation.  
**Keshav Singh:** right.  
**Rachik Sharma:** So for adults — somebody who is coming back and saying they're alive — we are okay to reverse the flag only. So for reversing the flag — same thing — we already had a flag which was used for marking the person as deceased — we'll use the same flag and pass the value `N`. And once this value `N` is passed — we'll check that particular UIN against the earlier marked flag — because in the records it  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** will have `Y`.  
   
 

### 01:11:40

   
**Rachik Sharma:** That way we'll get to know that it's a death reversal packet. And also we'll have this process — and there is no window for this because it's really hard to say how much time a person will take to come back and say he's not dead. There have been scenarios where people take five, ten years to come back and say he was actually alive  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** somewhere. Duplicate requests will be rejected. Then again it is sent to manual verification. Manual verification will do their due diligence. If they approve — then the flag is reversed. If they reject — flag is not reversed. These three scenarios are also to come later. Now one thing that has to be discussed further is the name of all these flags.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** And to clearly say which stages the packet will go through. Mostly it will not be many stages — because we want to send it to manual verification anyway.  
   
 

### 01:12:40

   
**Rachik Sharma:** There is no way we can verify whether the request is correct or not until we do it manually. So there will not be many stages — but still — we can have one or two — like a basic de-duplication saying that the request that has come in is not a duplicate request — something like that. And so all of this can be decided — but story-wise — in a theoretical way — we can still place these scenarios — rather than focusing on technical part — how to do it. Still — why we are saying that we'll deactivate — why we are saying we are not reactivating — and why we are okay with reversal of death flag — in what scenarios this can take place — those different things we can add as scope in the document under here — basically in this page we can have rare scenarios section — then we can decide what are the rare scenarios and explain why — are we saying it's allowed to deactivate for child but not reactivate — why there is no concept of reactivating — what is our recommendation — that kind of documentation we  
**Keshav Singh:** What is  
**Rachik Sharma:** can do. Technical part — I forgot to clarify — because technical part we can still not put because some things are not finalized — flow is finalized but tech design part is pending. So this is the whole scope — scope of this open CRVS and  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** CRVS and MOSIP.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Any questions  
**Keshav Singh:** Questions I was asking in between only — and we'll go through the docs once. Hello.  
**Rachik Sharma:** Huh? Okay. Fine. Keshav.  
**Keshav Singh:** Hello.  
**Rachik Sharma:** Okay. Okay.  
**Keshav Singh:** Right.  
**Rachik Sharma:** Okay.  
**Keshav Singh:** I'm getting another call. Rachik, I'll come back to you to this.  
**Rachik Sharma:** Stopping the  
   
 
### Transcription ended after 01:14:48

*This editable transcript was computer generated and might contain errors. People can also change the text after it was created.*Dec 17, 2025

## Meeting Dec 17, 2025 at 17:13 GMT+05:30 \- Transcript

### 00:00:00

   
**Keshav Singh:** Let's just talk in English then  
**Rachik Sharma:** I'm not sure.  
**Keshav Singh:** English  
**Rachik Sharma:** Uh, no problem. So, what we did? Uh, where was I? Uh, you want me to record the session or transcript is okay?  
**Keshav Singh:** record also. Can we do both at the same time?  
**Rachik Sharma:** Okay. So what what we did at that time was we uh uh we released this  
**Keshav Singh:** Okay.  
**Rachik Sharma:** uh uh platform to support the two major flows. One is child birth registration and one is child uh sorry one is child new birth registration and another one is uh any person who is deceased deceased basically death registration okay so what does it mean is cs will collect the  
**Keshav Singh:** H.  
**Rachik Sharma:** national ID of the parent in case of child birth I'm saying it will collect the national ID of parent they will authenticate it against e  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** signate make sure that the person is a legitimate citizen then they'll collect the details of the child they will verify everything that it's alleged birth.  
   
 

### 00:01:18

   
**Rachik Sharma:** So we are considering them as a source of truth. So this source of truth is one major point that has come across in many uh uh discussions and if you'll go through the documentation you will see that we have clearly noted it down at on multiple locations saying that we are we do not do any dduplication or data validation because we consider CRVS as a source of truth. Okay. So this is one major point that has to be there. So the idea is we consider them source of truth. We consider that they are doing their due diligence before sending us the request. Now once they send us the request what we have done for uh accepting their request is we have given them a we have opened  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** an API for them. And this is not only for them this is for any external partner.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Let's say tomorrow a passport service comes in and says that we want to uh send you the details of a person and we want you to uh create a UIN for that.  
   
 

### 00:02:07

   
**Rachik Sharma:** Okay. Or a driving license. Okay. any uh department which is external to MOSIP and wants to connect with MOSIP or access the MOSEP system we have given them a way of uh exposing this create packet API where they will act as a they will act as as a parallel to a registration client.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So currently packet is created by registration client but now we are saying huh now we are saying that source  
**Keshav Singh:** CL  
**Rachik Sharma:** is different from registration client source is CRVS in this case okay I'll share my screen one second so that I can show you what are the uh okay so this is the whole idea similarly for depth for depth What they were expecting was that we deactivate the ID but in most we do not consider deactivating the ID. I'll go into the details of why that is a decision made. But where what we do there is we ask them to uh send the request for a deceased person with their  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** UIN and then they submit the request and then we uh once it it is processed within MOSIP we add a flag saying deceased equals to true.  
   
 

### 00:03:23

   
**Rachik Sharma:** UIN is still active. Okay. Uh if somebody is using that UI and let's say somebody has add in my in my family and they have marked me as a nominee. Now I go to the bank to get all their savings. They will uh authenticate first using the ID of that person saying whether that is a legitimate holder in the bank account holder or not. So for that UIN is still activated because there are multiple services that can be tied to the same national ID of the deceased.  
**Keshav Singh:** Okay, got it.  
**Rachik Sharma:** But relying party will get a flag in a user info saying this is disease equals to true. So that will confirm the point that the person has died. So relying party and let's say somebody is trying to register for a new service using the same UIN. Then also they will get a flag saying this is a deceased person's UIN. Okay. Now it is directed to the relying party saying whether they want to give the service to the family or not based on the particular scenario.  
   
 

### 00:04:19

   
**Rachik Sharma:** So in MOSFE for death all we do is we add a flag.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Okay.  
**Keshav Singh:** Got it.  
**Rachik Sharma:** These are the these were the two major flows that we supported. Okay. Now coming to the D. So if you'll go through the first page you'll see what is it how it works why it is meta general uh uh theory about why the such integration is needed what are the principles what are the objectives those are written  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** here based on my understanding okay so this has to be overhauled okay building a story saying why uh such a need arises what are are different scenarios that a child might be required  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** to you know register for a UIN at the time of birth. How it helps and helps them. So basically by the time of birth you have a UIN that you can use uh for your lifelong services. Okay you want to get admission into school. You want to apply for child benefit services. The family wants to apply for child benefit services.  
   
 

### 00:05:24

   
**Rachik Sharma:** Now a child becomes adult uh then they want UI in through for the whole life  
**Keshav Singh:** Yeah. Mhm.  
**Rachik Sharma:** cycle uh you are sorted. if you have a foundational idea at the time of birth. Okay. So this whole page is talking about similar things. Okay. Streamlining streamlining processes. That's the main that's the point I was making. Secure and reliable data verification. Okay. Uh issuance of unique identity linked to birth certificates. Okay. Accuracy and identification. Now because by the uh at the very start of your life you are given a ID. Now the same ID can be maintained throughout. So similarly for deceased you can actually update the information for deceased when the person has died because that's the responsibility of CRVS. So we are also following the same. Okay. So this whole page is talking about those things. Okay. Once you will go through it you'll get to know okay what are we trying to portray here.  
   
 

### 00:06:19

   
**Rachik Sharma:** Okay. But it but it is written from my perspective. Okay. And this page has not been called out for any issues and all but still if some update or overhaul is required here we'll have to do it. Yeah. Now coming to the scope. Now scope as I already told birth and depth. Yeah. For each and there is one more scenario which is demo data update. We added it here earlier but the clarity was not there. Okay. So but now this also has to be updated. Now third scenario is let's say during the time of birth registration there was a issue with the uh name of the person name of the child or let's say when the child was registered uh uh there were parents now parents have died and child is getting adopted by someone and the surname has changed or name has changed okay later it was identified that the date of birth in the birth certificate is not correct they want to update that date of birth so coming to the birth registration.  
   
 

### 00:07:19

   
**Rachik Sharma:** Now since it's a infant or a child, they do not have biometrics. So in MOSP we have this fun of introducer and informant.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** We have this concept of introducer informant which basically could be anyone.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** So in MOS there is no uh relationship that is maintained between the person who is reporting the birth and between the uh child itself. Okay. For us it's always an introducer and we cannot uh you know collect the biometrics of the child because they their uh features are not yet developed. Okay. So in this case we are simply trusting CRVS saying that whatever uh you know dduplication and all you have done we are okay with it. Okay.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** This note here is to be modified saying here we have given that infants are individual between 0 to 5\. Okay. But this is not a definite rule. The demarcation between a child and a adult one can be driven from country policy. Okay?  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** It could be up to seven years, 10 years, whatever they want.  
   
 

### 00:08:24

   
**Rachik Sharma:** Second, if somebody uh when it comes to real use case, if somebody after a certain age has not updated their biometrics once they have become, they'll still fall under this category because their biometrics are not associated. Okay,  
**Keshav Singh:** Okay.  
**Rachik Sharma:** because for once a person's biometrics are linked with the ID then the expectation is all the changes or all the updates they want to do they do it using the biometric authentication. Okay. So slight uh slight change will come in.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** Okay. Now this is a simple uh step-by-step process. uh once you will go through it you will get to know nothing it's just that getting the details sending the request to MOSIP then MOSIP creates packet using the create packet manager API okay then you trigger the  
**Keshav Singh:** Amen.  
**Rachik Sharma:** workflow workflow trigger is nothing but to uh start the packet processing then once now it it goes through different stages quality classifier a uh demographic dduplication okay basic package structure check whether there is no uh you know issue with the way the packet is created.  
   
 

### 00:09:33

   
**Rachik Sharma:** Once it is done, we create an ID and we uh now here we have written that it's uh that CRVS has the capability to listen to this web subtopic where once the ID is created they can get the ID and if they want they can uh print it on the birth certificate or if they want for any other purpose or they want to link the birth registration number with the uh national ID they can also do that. So up till now this was something that we have written here but in today's discussion  
**Keshav Singh:** Excuse  
**Rachik Sharma:** it has come that this is a this is not a default uh setting for CRBS motiv MOSIP integration once the details are sent to the MOS MOS will simply create the ID and the  
**Keshav Singh:** me.  
**Rachik Sharma:** notification will go to the resident directly this case it could be parent okay uh since when we were implementing this there was a need in the country which is Tonga that they wanted to have this update given back to the CRVS with the credentials. We thought that we will make it as a default uh functionality but in discussion today it is clearly called out this is not a default functionality and should not be part of the default documentation.  
   
 

### 00:10:39

   
**Rachik Sharma:** It should be a separate section. If you want to get the credential back once MOSIP has done there is a way to do it but it's not by default.  
**Keshav Singh:** Got it.  
**Rachik Sharma:** Okay then another scenario is if dup duplicate request comes in.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** Okay, there are different scenarios for RI ID and all. We will once you'll go through the document, we'll come to the I think you'll have all these questions.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** What is RID and all? Okay, I'll explain them now also. But in general, you go through it. But the idea is since duplication check is done by CRVS. Okay, for these scenarios,  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** we are not doing any dduplication within MOSIP. Okay. So in case somehow a duplicate request comes in, okay, either MOSFE will override the existing data or they will create a new ID. So there could be a scenario if CRVS is has sent uh two different requests with the  
**Keshav Singh:** Okay.  
**Rachik Sharma:** same uh details of a child.  
   
 

### 00:11:41

   
**Rachik Sharma:** The child can have two also. That's a kind of a integration limitation.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Okay. And it is listed here also. If you go down integration limitations, uh duplicate request rejection. Okay, since CR source of truth,  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** MOSIP currently does not uh reject duplicate birth at request received from CRV system. This can result in multiple for the same infant or or an update of a death flag for the same deceased. Okay, dduplication is expected to be handled by CRV. So it's a so this is where the same point comes in that we are considering them as a source of truth. specifically for death and birth. Okay.  
**Keshav Singh:** Got it?  
**Rachik Sharma:** Okay.  
**Keshav Singh:** Because biometric is not there. This problem  
**Rachik Sharma:** Because biometric is not there for a child. Okay.  
**Keshav Singh:** is  
**Rachik Sharma:** Or for the deceased also. And this that is also reason. But another reason is CRVS is considered to be a legal authority in a country for all the vital events in person's life.  
   
 

### 00:12:47

   
**Rachik Sharma:** Okay. So ideally so what happens is when we said that uh earlier our stand was that if the duplicate  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** request comes in we'll add some validation from MOSP's side and packet can be rejected but their point of view was we are doing our due diligence we are doing everything why Mosep needs to uh you know do the same checks again and then do the if we are doing it we are the source of truth you simply accept the request okay so  
**Keshav Singh:** Mhm. Mhm.  
**Rachik Sharma:** they kind of were very uh rigid about this thing that they have to be the  
**Keshav Singh:** Uh so now the limitation uh uh is occurs because of that only right. Uh uh in case of child in case of  
**Rachik Sharma:** limitation is huh.  
**Keshav Singh:** child  
**Rachik Sharma:** So that's the huh one of the reasons for limitation because we are not doing any dduplication from our side and we cannot also because it's a new record in the system and also since there are no biometrics there is the only dduplication we can do is demo data.  
   
 

### 00:13:46

   
**Rachik Sharma:** Now demo data could be similar for two child. There is no way to actually verify because we are not collecting documents also.  
**Keshav Singh:** can be. Okay,  
**Rachik Sharma:** Huh?  
**Keshav Singh:** it can be  
**Rachik Sharma:** So even if our dduplication process gives out three or four results against a particular request saying that the name and the gender and the do is same there is no way to actually verify it. Okay. And in case of Banga since the population was very less we did not go into that direction also because it's a small country and there is no manual verification stays there that's so that's how they wanted it okay so multiple reasons but point is there is no dduplication as of now okay now for failure  
**Keshav Singh:** Okay.  
**Rachik Sharma:** handling uh if anything is failing within the uh within the MOSIP. Okay, we are not sending any notification to CRVS.  
**Keshav Singh:** All right.  
**Rachik Sharma:** We will only send notification to the resident directly. Okay, the reason for that is one reason is it's a foundational ID.  
   
 

### 00:14:49

   
**Keshav Singh:** Okay.  
**Rachik Sharma:** CRVS has no purpose of knowing whether the packet has failed or not. Okay, if there is any technical failure within MOS, we have retries, reprocessing functionality. So there is already someone who is sitting in the MOSFET. This is the argument that sassi made that there is always someone there is whole team who sitting on the MOSIP side and if there are technical failures we do have the provisions to reprocess the packet until and unless it is rejected or approved. Okay. So if it's under processing no need to send notification once it's approved or failed you send  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** notification to citizens directly because even if you send notification to CRVs CRVS actually cannot do anything person will have to come again. So person has to be notified saying that whatever request you submitted is rejected. You have to come again and uh submit a new  
**Keshav Singh:** And is there any UI for this?  
**Rachik Sharma:** request.  
**Keshav Singh:** uh Rajit like uh if uh once any data for adults uh for for the that matter you say like uh if uh data has been shared by  
   
 

### 00:15:59

   
**Rachik Sharma:** Mhm.  
**Keshav Singh:** CRVS that is updated through packet manager through API and gets updated with the MOSET  
**Rachik Sharma:** Huh?  
**Keshav Singh:** right if there is any change that has to be made right  
**Rachik Sharma:** Huh?  
**Keshav Singh:** uh to uh this adult's data on request or anything. So do they have any UI to do that also?  
**Rachik Sharma:** Mhm. Mhm. The only UI that will come in picture for any such record update is resident portal  
**Keshav Singh:** No.  
**Rachik Sharma:** UI. Okay.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** In this case and also like do not consider adult in this scenario. Okay. There is no adult scenario as of now. Okay. I I I'll explain in 2 minutes. So I'll come back to this. I'll finish this. So basically if something is getting rejected here we have written we will publish a failure event to web subtopic  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** detailing the rejection reason then CRVS will subscribe to that topic and get to know why the packet is failed.  
   
 

### 00:17:00

   
**Rachik Sharma:** All of these things will not be there by default.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** We will simply update the resident uh saying that we simply update the res saying that your request are rejected. you come again and register which is a very small scenario of percentage of packets that will get rejected because of such because we are not doing any validation on our side technically can it can only be something like Sashi was saying okay it goes to  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Abyis or it goes to some stage and it never comes out of that stage due to some technical failure then for that the SI who is responsible for running the MOSFET system in the country will do their due diligence and make sure that the system is working correctly. Okay.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** And also there is no uniqueness conflict that can be there in the validation. Okay. Because we are not doing redduplication. Okay. Mismatch can be there but it's not a valid scenario because schema is one of the prerequisites before you start this integration.  
   
 

### 00:18:03

   
**Rachik Sharma:** Okay. The same schema that MOSP is using has to be used for creating the packet in this case. So points are also not valid. Missing or invalid data, this could be a valid point but again it depends.  
**Keshav Singh:** Thank you Jesus.  
**Rachik Sharma:** Okay. So the can keep this but these two are not valid. Okay. So in general basically what we have to say in uh failure handling is uh technical failure is the main point saying that MOSB has the capabil ability to retry.  
**Keshav Singh:** That's funny.  
**Rachik Sharma:** Okay. We have given it here also. Retry mechanism automatically attempts to reprocess the failed request. Okay. Until and unless you have reached a stage of approve of approve or failed it will be once it has reached the stage where it says packet approved UI and created or packet  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** rejected UI and not created until then no notification to citizen also. Okay. Now similarly for death uh for death we introduced four four new fields in the ID schema.  
   
 

### 00:19:06

   
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Out of four we are only using these two as of now. Okay, which is declared as deceased. This is a boolean flag. And then deceased declaration date which is yet on which date the death request is coming  
**Keshav Singh:** So,  
**Rachik Sharma:** in. Everything remains same. Uh here in this case we'll get disease information. Again the person who is reporting the death could be anyone. It's for us it's inform informant. Okay.  
**Keshav Singh:** okay.  
**Rachik Sharma:** will validate both of their UINS and uh one more field  
**Keshav Singh:** Okay.  
**Rachik Sharma:** that has to be sent in the request is this e- signate user info token. Then this is applicable for both uh birth registration and death also. Okay. So e- signate user info token is nothing but if I am a parent registering for birth,  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** I will authenticate against e signit and then e signate returns a uh user info token. That user info token has to be passed in the request as one of the fields in ID schema.  
   
 

### 00:20:02

   
**Rachik Sharma:** Okay.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** Similarly for death who the person whosoever is uh doing the death registration sending a death uh you know and CRB sending us the request the person who is authenticated using the ID against e signate his user info token will be here. This is just an extra step to where the validation will come in from side to say that the person who has authenticated is a valid.  
**Keshav Singh:** This is for informment, right?  
**Rachik Sharma:** This is informant in both the cases.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** Okay. Then in death also they send us the packet.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** We trigger it. If everything goes fine the flag will be updated to yes death declaration flag and uh UIN will not be deactivated.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Now why UIN is not deactivated? That story has also to come here. Okay. So now we I'll stop at death. Now I I I'll say what is required to be updated here as a story part for both birth and death. Okay.  
   
 

### 00:21:06

   
**Rachik Sharma:** For birth we have to clearly call out here saying that the the biometrics are not associated for birth because a child cannot have a biometric. Okay. That's why we we allow all the packets from CRVS. Uh whatever packets are authorized by CRVS we allow them to come into MOSIP and we allow them to create the  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** UIN. We do not share any notification back to them by default. If you want to have this then it it's a separate section that we can give link here. Third thing is for death registration also. For death registration also why we are not deactivating the UI and uh automatically is because once a person is deceased there are multiple services which are still associated with the ID of the person. the family or the closed ones of the family who wants to access the  
**Keshav Singh:** Okay.  
**Rachik Sharma:** services of the deceased UIN will still require the UIN to be activated. So it from foundational ID system point of view since it's a interconnected system and it is being used by  
   
 

### 00:22:04

   
**Keshav Singh:** Okay.  
**Rachik Sharma:** downstream services left right center we cannot de automatically deactivate it. Okay. So this this that is the point.  
**Keshav Singh:** Example could be bank uh thing. Example could be like uh insurance.  
**Rachik Sharma:** Insurance insurance insurance is one major example is insurance.  
**Keshav Singh:** Insurance. Yes. No.  
**Rachik Sharma:** Then bank is one example. Then there could be a scenario where you want to let's say you use your ID to uh pay monthly bills somewhere. Okay. Now you want to change that ID.  
**Keshav Singh:** in now now as in case of like uh Indian uh what uh land registry systems also certain states have started using Aadhaar right so likely  
**Rachik Sharma:** H now let's say if a land has to be transferred and you have the mobile of the deceased you get the OTP there you authenticate and then you put in a request to change the ID first you prove that it's the ID of the person uh with me and then you submit a request for ID change and it can take longer than expected to change the ID.  
   
 

### 00:23:15

   
**Rachik Sharma:** Okay, but until then you should be able to claim your uh right to the land now.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So for those all these various purposes this has to come out as a story why we are not deactivating UIN directly deceased persons. Okay. Now third scenario comes in from demog demographic data update.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Now there are two scenarios here. One is it could be a demographic data update for an infant. Okay. Let's say within the let's say country has decided that first five years uh 0 to five is the age which we consider any request to be child above that we expect them to be adult means biometric has to be there. Okay. So if any request now first thing is if any request comes above that age that validation we have okay because in because in MOSIP any packet that comes in we uh we uh pass them through this  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** child filter where we validate whether the age of the packet based on the date of birth given in the packet whether the age of the packet is greater than the configured age or less than the configured is less then we allow the packet.  
   
 

### 00:24:20

   
**Rachik Sharma:** If it's more that is  
**Keshav Singh:** And that a that age is configurable,  
**Rachik Sharma:** configurable by default I think it's 5 years in MOS but it's up to the  
**Keshav Singh:** right?  
**Rachik Sharma:** country. Okay. So currently also in the integration there is no support for new adult birth registration. Let's say today in the country the integration starts and I go to the CRVS system saying that I'm 27 years old or I'm 32 years old or 35 years old and now I want to register for a birth registration. they will not send us the request for UI and creation because we will reject it because it's a adult. If you are an adult, you need to give a biometrics to register yourself. So that logic is already there.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So there is no new adult birth registration as of now.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Similarly for demographic data update also for demographic data update one scenario is infant or pre-biometric stage. Okay. So we can so the story has to come out here is that any person uh who is under a certain age as per the country policy and biometrics cannot be  
   
 

### 00:25:16

   
**Keshav Singh:** Mhm.  
**Rachik Sharma:** collected we consider them as infant but it does not limit to that okay if do not  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** have a biometric linked to the ID that's the second point then also we can consider that as a demographic data update for child although they are not child because they do not have a biometric associated so if they do not have a biometric associated which in larger scenarios will  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** always be an infant but in some scenarios it could be someone who is not an infant but let's say a minor 7 or 8 years old and they do not have biometrics in that case also they can CRVS whatever request CRVS sends we will take that request based on the correction that they want to do correction is not limited to but suggestion is to have name DOB and gender Okay. So this list is not exhaustive.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Uh this list is just suggestive. Okay. I think we have a note somewhere here.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Okay. The list is suggestive.  
   
 

### 00:26:21

   
**Rachik Sharma:** Huh. Here supported demographic fields for update. We say name data or gender. This is a suggestive not restrictive. Countries can configure any demographic attribute. So if countries decides okay from CRVS we should allow the address update and they want to add that field in the update we are okay with it.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** It's up to them and but it should be a ID which does not have a biometric linked. Okay. Or if country wants to decide using the age then it should be an infant. Then only this update will take place. Okay. Second thing is IDs which have biometric linkages. It could be adults or individual enrolled with biometrics into the ID system. Okay. For these we are not doing any uh demographic update. For these user will have to go to the MOSFE registration center for updates the basically ID system. Okay. Because once you have your biometrics linked it is the most assure  
**Keshav Singh:** Mhm.  
   
 

### 00:27:27

   
**Rachik Sharma:** assured way of proving that you are the one who wants to Oh one second.  
**Keshav Singh:** Is it washroom? Come. Uh, please. Yes, I see.  
**Rachik Sharma:** Uh one second. Uh cash off.  
**Keshav Singh:** Hello.  
**Rachik Sharma:** Uh okay. Huh. So what I was trying to say was okay now re biometric is the only way to actually assure that you are the person because there is no better way than biometrics and there is no way to send the biometrics for CRVS that kind of uh design is not in place. So if it's an adult and biometrics are linked with it there is no point in sending request to MOS for any update. The reason one reason is this that it has the highest uh assurance uh high highest assurance level that we can achieve for for any update. Second reason is again the same that the person who going to the CRVS uh thinking that  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** they'll do a name change from CRVS site does not understand the impact that once a name is changed in the national ID also they might start facing issues.  
   
 

### 00:28:45

   
**Rachik Sharma:** Let's say in passport they have a different name. Now in ID their name is different. Okay. They'll start facing that issue when they start accessing the services. So the implications might not known to the citizen itself that updating a name change from the  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** CRVS might result in many different issues. Okay.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So if you really want to do any name change or gender change or do OB change or any other address update change, you should go through a proper biometric process within MOSIP ID system where you can be sure that whatever changes you are making will not result in interrupted services. Okay. So there is that's why there is no point in getting a request from CRVS for adults.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Okay. And it's not  
**Keshav Singh:** for demographic change for uh especially in case of name change and so what are we uh telling it uh to CRVS like are we accepting any such request or not  
**Rachik Sharma:** No no no no we are not we are not we want to associate  
   
 

### 00:29:50

   
**Keshav Singh:** okay  
**Rachik Sharma:** so that's where the documentation overall will come in where for adult demo data update which is this point uh point number two Okay, for this point uh okay uh this is for child  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** and then this point 3.3 okay there has to be a story to actually rationalize and put out the risk so there could be a risk section  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** here okay to rationalize I've already given but yeah we'll have to make it in a certain section saying that if you allow such kind of updates for adults from CRVS these are the risks that you can run into. One risk is it's not the best way to authenticate an individual who already has a biometric linked. Second is multiple doms downstream services can be affected. Okay. Essential services uh because of this demographic data change.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Okay.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Now uh citizens themselves uh and second thing is uh foundation. So, so ID life cycle or ID update is actually the part of ID department not the CRVS department.  
   
 

### 00:31:05

   
**Rachik Sharma:** Okay. Name change is not a kind of a vital event that CRVS system should be worried about.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Okay. In cases of child,  
**Keshav Singh:** Yes.  
**Rachik Sharma:** in cases of child we are okay because they are the most authentic source of truth because they are the ones who are actually collecting the documents. Okay. We do not do that. But in case of adult if biometric is associated that means that person now is part of ID system properly.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Now to manage the ID should be done from ID system not from the CRVS system because uh technically it's not part of there.  
**Keshav Singh:** Got it.  
**Rachik Sharma:** So it has not to be put in this way that CRVS has no legal authority. They have legal authority. It's fine but it makes more sense and it is more secure if person's ID is managed from MOSIP ID or nationality department okay that biometric reauthentication can happen if they are submitting a uh you know document for let's say name  
**Keshav Singh:** Um,  
   
 

### 00:32:00

   
**Rachik Sharma:** change currently we are not getting any document from CRVS they can show that document in the national ID department that okay  
**Keshav Singh:** right.  
**Rachik Sharma:** he see here is a approval of my name change now I want to change my name so the same can be notified the multiple downstream services. Okay. So there has to be a risk associated  
**Keshav Singh:** Means means what I understood h what  
**Rachik Sharma:** section. Hello  
**Keshav Singh:** uh a national ID system has rules defined and ideal practices which should be followed and uh country implementing national ID system like MOSIP or any other uh and the citizen of that country should follow even if there are name changes and there are different uh these practices of name change say or demographic change uh to passport or to other services pan or something. So uh that should be to avoid that to avoid that uh national ID system should not accept or massive should not accept uh what uh demographic change from CRS and it it is not advised or uh  
**Rachik Sharma:** for the for the adults or for the people who have biometric linked in that  
   
 

### 00:33:26

   
**Keshav Singh:** yes  
**Rachik Sharma:** and the point is it has to go as a recommendation. So it's not a hard and fast rule. Okay. So when somebody reads it, it should clearly say if you do this, these are the risk you can encounter which  
**Keshav Singh:** Mhm. Like in India also like if you see in India also taking example from the best uh uh  
**Rachik Sharma:** might  
**Keshav Singh:** national ID system implementation right here also when uh uh PAN card would be there was a talk of P linking the PAN card there was uh what uh people raising concerns about it about it or anything whenever there is a uh there is a request or there is a uh there is there's something comes that you should link anything to Aadhaar. There is a concern uh raised and a concern raised  
**Rachik Sharma:** Huh? Huh?  
**Keshav Singh:** because that uh what is advised is you have different identity different uh thing for c uh entirely different purpose. Pan card you have different purpose, passport you have different purpose. So if if if the request is even coming from  
   
 

### 00:34:37

   
**Rachik Sharma:** Yes.  
**Keshav Singh:** CRVS to update the demographic data to national ID system, it is a risk. first thing. Second thing, unless the country has uh even uh linked every other services or uh ID uh thing with national ID which is again what a huge task for country and uh decision making of of the country also that whether they will go for it or not. In that case also uh there are chances that no country will link all the services to national ID again.  
**Rachik Sharma:** Huh? Understanding wise it's correct. But that we do not want to put here.  
**Keshav Singh:** Huh.  
**Rachik Sharma:** What we want  
**Keshav Singh:** Understanding wise it is correct.  
**Rachik Sharma:** to  
**Keshav Singh:** But uh just to articulate we have to different uh we have to articulate different points here which should be valid in case of this particular document  
**Rachik Sharma:** Huh.  
**Keshav Singh:** and integration uh point of  
**Rachik Sharma:** Huh?  
**Keshav Singh:** view.  
**Rachik Sharma:** So here what so the idea is here we what what we want to say is that it's first thing is it's a recommendation that any country should not allow adult or ids with biometrics already  
   
 

### 00:35:55

   
**Keshav Singh:** Mhm.  
**Rachik Sharma:** there should not allow demographic updates directly from CRVS. This is in one line is the recommendation. If you do this, these are the list of risk that you can encounter. After that, there can be a note saying even if the country wants to do it,  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** it's only a configuration and country is free to do it. If they if they deem it necessary that it has to be done from CRS system.  
**Keshav Singh:** Like in that case also should we not recommend that it should be uh national ID. If you have implemented national ID then in that case also like uh if you want to update the demographic data you should uh be updating it through the uh uh national ID system or foundational ID system not the  
**Rachik Sharma:** Huh that's that's the recommendation that you should not allow from CRVS but instead citizen should  
**Keshav Singh:** registry.  
**Rachik Sharma:** visit MOSFEP ID system. This is the recommendation. Why it is a recommendation will be explained in uh the risks that can be associated with direct updates from CRVS for such cases.  
   
 

### 00:37:15

   
**Rachik Sharma:** Even after that if somebody wants to go and say no no no no I have uh you know covered all the corners and I in my country this will not be an issue I want to do it from CRVS then it is like it's their decision at the end it's not a integration limitation it's a it's it's it's just that by default it will not be there in integration if you want to do it you take the code you customize it and you do  
**Keshav Singh:** practices recommendation. And it is what we are giving. It is not the tech limitation  
**Rachik Sharma:** No, it's not a tech limitation. Huh? It's not a tech limitation.  
**Keshav Singh:** platform.  
**Rachik Sharma:** It's a uh most recommendation or what we feel necessary that countries should know before making any such decision. Okay.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So that has to come here for adult demo demo data update or basically saying that demograph demographic update for ids with biometric linkages. Okay. And all these whatever I've written this has to be elaborated in a way saying that okay this could be a scenario where you want to do this but we do not recommend it do not do it directly from CRVS visit this why you should visit it refer below the risk associated with the direct demographic update for an adult and why we suggest that you should come to national ID then you list down the risks then you say even if after  
   
 

### 00:38:34

   
**Rachik Sharma:** that a country decides that they want to go ahead with this this is this particular feature is not part of the default integration but there is no tech limitation you can take the code customizer according to your needs and go ahead with the adult updates as well and similarly based on  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** the whatever changes we'll do in the above section integration limitations the headings will be changed okay it will something are limitations like dduplication check is a limitation we are not doing it okay packets packet status of rejected or upd is not given to CRVS it's a limitation ation or we can say not supported or features not supported something like that rather than limitation. Okay. For similarly for offline integration there is no scenario for offline integration you have to do the once you have collected the data. So what so one thing for offline also I would say that we should have a separate section which has come recently is one is although it is it is written here that integration only works when online connectivity is available as e signet authentication is a necessary step before a request is submitted by CRVS not CRVS before a request is submitted by CRVS offline no connectivity support is under development we have said but this has to be removed there is nothing under development for offline.  
   
 

### 00:39:58

   
**Rachik Sharma:** Okay. The scenario is why this has to come in the picture is CRBS was saying that there there are scenarios where they go to remote location let's say in villages and all the field agent who is going there collecting the data and at that point it is not possible to get the authentication of the parent or informant who is the details of the child for birth registration. Now for such scenarios we should be able to send the uh request directly or we have some offline mechanism to do that. So our recommendation here is or our stand here is no matter that you are in a remote location let's say and you have collected let's say 100 records okay  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** hello  
**Keshav Singh:** Yes.  
**Rachik Sharma:** Huh. Now you have gone to a village you have let's say collected 100 records for children. Okay with name and UIN of the parents. Okay. Now when you once you are going to any CRVS center where the online connectivity is there because to send a request you need online connectivity because you are an API.  
   
 

### 00:41:23

   
**Rachik Sharma:** So in that case since you are not able to authenticate parents the person the register  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** the admin person who is sitting at the CRVS center and approving the request  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** all these 100 records can have the e- signet authentication of that particular person because we consider that he's a legal entity sitting there what need is we need the e signet user info token of the person who is approving the  
**Keshav Singh:** Okay.  
**Rachik Sharma:** request in case you are not able to authenticate parents Okay. So that's why there is no support for offline integration. Even if you are collecting data offline, you come online, you authenticate you uh with the ID of huh yourself or the ID basically ID of the person who is approving  
**Keshav Singh:** Yes.  
**Rachik Sharma:** the request. It could be a register. It could be a CRVS agent who is legally assigned to that role of approving the request and sending it to MOSIP. But we need that is e e signate user info token because we want to know from where the request came or who approved these request because tomorrow let's say thousand or  
   
 

### 00:42:25

   
**Keshav Singh:** Okay.  
**Rachik Sharma:** 10,000 records for birth let's say they were illegitimate somebody was just registering children to get the UIMS and get the services in a country and audit comes in e sign uh Mos should be able to say okay see this is the person who approved all these requests okay so no offline connectivity activity is there. So this is the logic that uh for MOSIP's audit and MOSIP's purpose for MOSIP's info we just need the ID or user info token of the person who is approving the request in case the people are coming to center where there is online connectivity. Well and good you take the uh authentication you do the authentication of the informant of parent and send us the e signet user info token. In case you are collecting data offline and then you are going to center where there is connectivity, you ask the register to do it. Okay.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** So that's one thing.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So this is the uh so overall as of now this is the scope that is written here and it has to be modified in this storytelling of way where we justify each and every action that why we are recommending something and nothing is a technical limitation.  
   
 

### 00:43:40

   
**Rachik Sharma:** Okay, apart from some steps like dduplication, we are not doing offline uh authentication. There is no way to do offline authentication. Okay, other than these two three points, most of the things are recommendation that MOSIP does. Okay. Similarly, once so this page will be overhauled and the structure also like if you see here the way it is written, it's kind of confusing is now what I feel because people are facing a lot of issues. People are not facing a lot of issues but new requirements and coming in and um some points might not uh you know address or  
**Keshav Singh:** Thank you. Uh to to that I'll have some suggestion uh Rajik and uh uh but before that  
**Rachik Sharma:** uh  
**Keshav Singh:** like do we have anything else like approach uh is the next topic?  
**Rachik Sharma:** so next is approach. Okay. Now how we are enabling them to do this? Okay.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** very clearly it uh gives us the prerequisite that okay it is on a high level it is clear that we have opened a API from packet manager so that you can directly hit that API and create a packet instead of going through registration client so basically you become kind of a online registration client for us  
   
 

### 00:44:51

   
**Keshav Singh:** Mhm.  
**Rachik Sharma:** okay so that packet can be created and uh and it can be processed so to do that what is required is clearly written here okay that you create you need to create a new client assign a role  
**Keshav Singh:** H  
**Rachik Sharma:** obtain the tok token for API because this is the API call that you will make to some.  
**Keshav Singh:** I lost you.  
**Rachik Sharma:** Hello.  
**Keshav Singh:** Hello.  
**Rachik Sharma:** Hello. Hello.  
**Keshav Singh:** Yes. Yes. Now I I can hear you  
**Rachik Sharma:** Okay. So here list of steps is very clearly written that what is  
**Keshav Singh:** now.  
**Rachik Sharma:** to be what is required to be done even before you submit a request to MOSIP. Okay. Uh similarly like it's very similar to what we have in R client. So if somebody's setting up a Mosibet system in a country,  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** these are the steps that they'll do. They'll create a role, they'll set up a registration center, they'll request a machine, uh they'll register a pro officer profile, they'll map that center to the machine.  
   
 

### 00:45:48

   
**Rachik Sharma:** Okay, then every ID that is every request that is generated for UI and creation will have a application ID.  
**Keshav Singh:** All  
**Rachik Sharma:** Okay. So, same thing CRVS will have to do because now they are becoming a kind of online registration client and it and this all work falls in their kitty kitty or SI's kitty whosoever is implementing it. Now once everything is done you uh send the request to the create packet op uh API and you uh basically submit data using create packet  
**Keshav Singh:** right.  
**Rachik Sharma:** API upload the packet to MOSEP then trigger processing validate then generate identity credential then these seven or eight points will not be here because these two points are ex exclusively talking about a web sub event that will be published so that CRVS can get the update. This is not part of default. If you really want it, this will become a separate section.  
**Keshav Singh:** So was it a point of conflict that happened recently? Uh they were demanding something like this uh to be default or  
**Rachik Sharma:** Huh. So earlier when we did this release at that point it all of us were in a confidence or  
   
 

### 00:46:55

   
**Keshav Singh:** what  
**Rachik Sharma:** at least under the apprehension that everyone is agreeing to providing updates to CRVS. Okay. So at that time since Tonga was a country we asked the SI to register or to subscribe to a particular web subtopic and as of now they only get the credentials for the packets that are approved and UIN is created. Then they came back and saying okay what about the packets which are getting failed. We also want to get the CR said we also want to get the updates for the packet that are getting failed because then we'll get the info and then we can uh you know uh resubmit the request with the corrections. So that's why we were thinking that this will become part part of default but it is now clarified that this will not part of the default because CRVS has nothing to do with the identity credentials. Their only purpose is to submit the info to us and then what we do on MOSIP side is between us and the resident.  
**Keshav Singh:** Yeah.  
**Rachik Sharma:** If  
   
 

### 00:48:01

   
**Keshav Singh:** And if if if uh for that matter if they have done the due diligence and they  
**Rachik Sharma:** if  
**Keshav Singh:** have created the records and then the packet manager starts uh means their API  
**Rachik Sharma:** Mhm.  
**Keshav Singh:** their record hits as the packet manager uh APIs and that flow is initiated. So no point them worrying about it even post that is it in case of failure only.  
**Rachik Sharma:** uh it's not related.  
**Keshav Singh:** Yes.  
**Rachik Sharma:** H uh in case of failure also let's say CRVS has registered a request  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** now they have created a birth certificate and they have given it to the citizen. Now request comes to MOSFE and it is stuck in MOS let's say for 5 days due to some technical issue and at the end it gets failed. Now what will CRS do in this?  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Now what citizen has to do is they have to resubmit the request for UIN right so that's why CRA should not be concerned about uh the update of a particular packet because uh there is no dependency on that they do not have any dependency okay but even if they want to do it then there is a way to do it but it's should not be part of a default integration this has to very clearly come out in a separate section saying that  
   
 

### 00:49:17

   
**Rachik Sharma:** Why we do not want to send notifications to the CRVS?  
**Keshav Singh:** Yes.  
**Rachik Sharma:** Why we think that from foundational ID perspective it is not necessary for CRS to be updated on this because first there is no dependency. Second is uh citizen CRVS there is nothing CRVS can do to resubmit the request. Citizen has to be informed to resubmit the request and that notification anyway we send to the citizen saying that there is there was a request for a new ID creation and it is now failed. You have to come again to the center and resubmit the request. And third is the percentage of such packets getting failed and UI not generated is very less because MOSIP already has a way of internally reprocessing the packets if there is a technical failure. Fourth point is uh there is no data validation actually that MOSIP is doing.  
**Keshav Singh:** I'm moving  
**Rachik Sharma:** So most of the scenarios will always be technical failures within MOSIP for which the  
**Keshav Singh:** this.  
**Rachik Sharma:** national ID department is responsible to make sure that all the packets go through the reprocessing and they reach the end stage of approved or rejected.  
   
 

### 00:50:26

   
**Rachik Sharma:** Okay.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So these three or four points we can list down saying that why we do not want to send the notification to the uh why we do not want to send the notification to the uh CRVs system specifically but instead to the  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** citizen. Okay. So these two points will be removed from here and there should be another section for notification saying that to whom it will be sent why it will not be sent to CRVS what is our recommendation. Okay. and why it is not supported by default.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Even if they even after that if somebody decides no I want CRVS to be updated then there is a way to do it  
**Keshav Singh:** invitation.  
**Rachik Sharma:** which is using this web subtopic okay okay so that's one thing that has to be changed here then coming to technical details now here uh in technical details it's similar basically whatever I have written in approach I have expanded and elaborated it here create how do you create  
**Keshav Singh:** Mhm.  
   
 

### 00:51:25

   
**Rachik Sharma:** a CRVS role Okay,  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** this is this all I don't think we require we need to change because standard process all of this will same only thing that I want to explain you for your understanding is step eight which is create API okay which is not coming out here  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** very specifically. So whether we want to put it in the approach or in technical details, I think it should be in the approach because one thing that actually determines whe  
**Keshav Singh:** All right.  
**Rachik Sharma:** which which camel route or basically which stages in the registration process or packet will pass through totally depends on this process and source. We do not have separate APIs for different flows like we do not have another API for birth and another API for death and another API for update. using the same a this same API request structure based on the value that is given in the process.  
**Keshav Singh:** Huh? So this API uh documentation should be improved. No, right? Means hello. Huh?  
   
 

### 00:52:31

   
**Rachik Sharma:** Uh-huh.  
**Keshav Singh:** making it a condition like uh condition it and explaining it in the very beginning of these uh two things process and source and then explaining uh all other parts of uh this API mean here we have just put the API  
**Rachik Sharma:** uh API document is here we have put the  
**Keshav Singh:** right sample  
**Rachik Sharma:** request it's a sample request we  
**Keshav Singh:** request and have we explained that uh  
**Rachik Sharma:** have explained that also what are the different what are the  
**Keshav Singh:** mhm  
**Rachik Sharma:** different feeds that are given here okay so the whole basically the whole point I think that should go in approach is that whether packet is considered considered as a birth  
**Keshav Singh:** Okay.  
**Rachik Sharma:** registration packet or packet is considered as a death registration packet or in future a packet is considered as a update packet. All of this is decided by post uh process and source combination. Okay,  
**Keshav Singh:** So it can have different value right.  
**Rachik Sharma:** source source will always have the same value because it's coming from  
**Keshav Singh:** Okay.  
**Rachik Sharma:** single CRVS system.  
   
 

### 00:53:37

   
**Keshav Singh:** Right.  
**Rachik Sharma:** Okay,  
**Keshav Singh:** Okay.  
**Rachik Sharma:** process will be different.  
**Keshav Singh:** And  
**Rachik Sharma:** So for birth it will be CRVS new.  
**Keshav Singh:** process.  
**Rachik Sharma:** For death it will be CRVS death. For update it will most probably be CRVS update. Okay. So based on the process and source we decide how the packet is treated and which stages go through. So CRVS do not have to be concerned about different set of APIs for each different flow. It's a single. So that thing is not coming out very clearly. I think we can add this in approach.  
**Keshav Singh:** So which means if there would be separate uh requests right uh which in ideal case of Mossip and registration client would be happening different APIs to do those things. Here we have single API in which there are two conditions listed above and the process and its value will define what uh how it will be processed by the packet manager.  
**Rachik Sharma:** by the pack by the registration processor and how MOS and how MOSFEP  
   
 

### 00:54:39

   
**Keshav Singh:** Right? Registration processor. Okay.  
**Rachik Sharma:** will identify whether it's for birth or death or any other workflow. So in general what has to come out is that this process value is very important. Whatever process value you give packet will be created as per that and will be processed as per that. Okay. Then there are different uh you know fields which are uh already given like here we have given when initiating a infant birth request when initiating a death registration request. Now tomorrow if we start supporting update then it becomes CRVS updates and we'll say when initiating update request for infants specifically. Okay. So that has to uh come out clearly. I think approach is the right way because here if somebody comes and sees they'll get they'll think that there are only uh two supported processes. Okay, in the approach we can list down the supported processes based on the tech design and all that we can have a discussion but this has to be updated and uh that's it. So this is the last page technical details.  
   
 

### 00:55:47

   
**Rachik Sharma:** Okay. Now adding to the scope.  
**Keshav Singh:** Got it.  
**Rachik Sharma:** Now things that will get added to the scope are these things. Now this is the next part which we have discussed recently which is this. Okay which we are calling as rare scenarios. Okay. So there are three rare scenarios. One is uh let's say CRBS has submitted a request and five uh days later 1 month later they realize that oh the document that was given somebody comes and tells them that this birth that was registered is a fraud birth or all the births that that are having the birth document or birth record for this hospital are all fraud and CRVS decides to send us all these requests saying that  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So by mistake there was a fraud or somebody uh you know it can be incorrect actually fraudulent is not the right term.  
**Keshav Singh:** Mhm. Mhm.  
**Rachik Sharma:** Fraud is one of the scenarios but at the end is CRS realizes that whatever birth request they have given or  
**Keshav Singh:** Mhm.  
   
 

### 00:56:50

   
**Rachik Sharma:** what whatever birth certificate they have assigned is not a valid one due to some data discrepancy due to some other misinformation and they want to nullify that  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** record from their side. they can do directly. They can know nullify the birth certificate and they can ask them to create a new birth certificate with the correct details. Okay. But they want us also to deactivate the ID. Okay.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Similarly for reactivating an ID which is a stupid scenario but still they came up with it. I mean uh not stupid scenario but I mean it's a very uh illogical scenario because once you have deactivated why would you want to reactivate? And this is all uh coming from the point first of the two scenarios are coming from  
**Keshav Singh:** Thanks.  
**Rachik Sharma:** the point of view of birth registration okay that it was a fraudulent birth we deactivated now again later somebody came with a court order saying no no it was a right birth now again reactivate the ID it doesn't make sense we have a we have decided how we will deal with it but that's what CRVS was expecting and third is reversal of death flag basically somebody was mistakenly uh entered as a deceased.  
   
 

### 00:58:04

   
**Rachik Sharma:** Now one year later uh he was in a war on war torn country on the battle front. Now 6 months later or one one year later he comes back home and he he gets to know that oh I am marked as deceased. He goes to the CRV saying that see I was alive. I am the person I am. Please uh you know give uh nullify my death certificate and ID system should also reflect the same.  
**Keshav Singh:** In fact, I I think for fun uh fun in intended, I think these are all valid scenarios. even reactivating how one example just I was reading on social media  
**Rachik Sharma:** Mhm.  
**Keshav Singh:** like uh somebody conspired against somebody to get his insurance money right and all those document documentation and  
**Rachik Sharma:** Mhm.  
**Keshav Singh:** uh verifications and uh everything has been done like uh death certificate and he was declared dead and now he's fighting for his uh cause like uh to establish themselves alive again, right?  
**Rachik Sharma:** Huh. So that is a valid scenario. Okay. But that will come as a uh that will not be a valid scenario for  
   
 

### 00:59:13

   
**Keshav Singh:** So,  
**Rachik Sharma:** MOS because we actually do not deactivate any ID for adults. These two are specifically for birth scenarios. Okay.  
**Keshav Singh:** okay.  
**Rachik Sharma:** So from birth scenario perspective if you already deactivated something why would you want to reactivate it  
**Keshav Singh:** It says yes.  
**Rachik Sharma:** again?  
**Keshav Singh:** Correct. Correct.  
**Rachik Sharma:** That means that in first place your deactivation not was not  
**Keshav Singh:** So when they come up with this kind of request do they uh provide the reason for it  
**Rachik Sharma:** proper  
**Keshav Singh:** like uh scenario for it in what kind of scenario it is valid do they provide?  
**Rachik Sharma:** I I'll come to that. Okay. So these are the three different scenarios that have to be added to the scope now.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** Okay. Now, first thing is uh this one fraudulent birth or incorrect birth. The word do not take the word directly from here. It's it should be incorrect birth or mistaken birth or something like that. But now what we are proposing it since we want to the reason now first thing is why we are supporting this.  
   
 

### 01:00:21

   
**Rachik Sharma:** Okay, we are supporting this because again this is a birth scenario. CRVS is doing all the document verification from their side. If they are finding something that means it's a legit information that they have found and it should be passed on to foundational ID system.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Okay, that's why we have we are considering this scenario. Now when they are sending us the request since there are no biometrics involved we allow them to send the request and we allow them to uh raise a request for deactivation but we'll not do it directly. Okay. What we will do is similarly to deceased flag we'll add a new flag saying deactivate ID that will be part of ID schema which they'll mark it as true.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** Then a new process will be defined which will clearly mark this packet as that this is for deactivating a child ID. Okay. Then we will ask them a reason. It can be a string. Okay. They can give any reason. Okay.  
   
 

### 01:01:20

   
**Keshav Singh:** Mhm.  
**Rachik Sharma:** It can be that this is due to fake birth uh document from the hospital. the child is actually not born, it's a fraud. Anything they can pass, we take that. Oh, and any optional supporting document that they want to say, any court order or uh an actual uh the fake uh medical certificate or hospital certificate that was there. Okay,  
**Keshav Singh:** Will will do we store this uh document  
**Rachik Sharma:** we if they are passing it,  
**Keshav Singh:** somewhere.  
**Rachik Sharma:** we will we will not store it but we will keep it uh with the packet for manual  
**Keshav Singh:** Okay.  
**Rachik Sharma:** verifier because this packet will ultimately go to manual verifier. Okay. Similarly, now this request has come with all the details along with the other details that I was showing you in the API request.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** Most probably the API request will be updated for this to include all these fields.  
**Keshav Singh:** The same API is  
**Rachik Sharma:** Uh mostly it will be the same API that will be updated to include all these fields.  
   
 

### 01:02:25

   
**Rachik Sharma:** So the all these fields will if they are filled then uh you know we'll just basically validate okay it's a deactivate packet and this is marked as true that mean it's a uh child deactivate packet and now it will be for given to manual verification. Moz will not do anything. Okay, because it will need to have a UI in of that particular child also. Okay, we have a similar thing for death and birth also. We follow the same API request structure. The different flags are there. You update the respective flags for okay now if uh  
**Keshav Singh:** Love  
**Rachik Sharma:** now there is a window it's all again country decided.  
**Keshav Singh:** it.  
**Rachik Sharma:** Okay, we have to when we write the story we have to give a reason why we are suggesting a period of 60 or 90 days. One reason is uh in this uh in this time period it is very uh it is possible that the ID that is created for child is not being used for uh many essential services. Okay.  
   
 

### 01:03:24

   
**Rachik Sharma:** Because child is just born. The documents in the hospital are freshed for referencing.  
**Keshav Singh:** Amen.  
**Rachik Sharma:** Okay. It also gives a window for a early kind of a incorrect or fraud detection. Okay. So that if CRVS knows that this is a time window or country decide that this is a time window, there will be a due diligence to make sure that every birth request is uh you know revalidated in case somebody comes in. Okay. And population will also know that if uh I have to submit a request for deactivation, I have to go within a window. So they will be uh uh you know uh do their due diligence to make sure that if something is reported wrongly, they are coming. But it is again again a country decision.  
**Keshav Singh:** Hello.  
**Rachik Sharma:** It's a configurable window. No hard rule. It's up to the country. They can keep it as one year, 2 year, 5 year, whatever they feel uh you know relevant for their case. Okay.  
   
 

### 01:04:23

   
**Rachik Sharma:** Okay. So uh now based on this date of initial registration and based on the current date we can decide whether it's within a window. If it's within a window, fine. If it's not within a window, still we will accept accept the packet. Okay, this window is just for the manual verifier who will verify this request saying that see this  
**Keshav Singh:** Okay.  
**Rachik Sharma:** request has come these many days later. Okay, just a kind of a information. Okay. Now,  
**Keshav Singh:** Okay.  
**Rachik Sharma:** if within a window MOSFE flags the record, flags as in uh this uh basically this flag is already coming in with the request. Okay. deactivate ID equals to true. Now AIC dduplication will happen and it will be directed to directly to the manual verifier stage. Okay. Okay. And then uh manual verifier will see the reason we'll see any supporting document which is there. They will make their own decision. they will have the they if they want they can offline connect with the CRVS or the hospital or the parent to verify why this is a illegitimate birth.  
   
 

### 01:05:32

   
**Rachik Sharma:** Okay, that all those checks are now left to manual verifier. What MOSIP uh is doing is they are rerouting the packet to the manual verification stage. Okay, once the and if any duplicate request comes for the same UIN, we will reject it because we can check whether the flag is already true for that UIN after the last request or not. Okay, basically duplicate request will be rejected and then uh manual verifier of the country will uh decide whether it's true or false uh whether is approved or rejected. If they approve that means they are okay with deactivating. Uh a logic will be added within MOSUP to automatically deactivate the UIN.  
**Keshav Singh:** H.  
**Rachik Sharma:** No manual no manual step.  
**Keshav Singh:** Okay.  
**Rachik Sharma:** Earlier we were thinking that we will leave it to the country officials to do it from back end but what SEI is saying is we deactivate it directly if they are approving it because manual verifier is making a decision. So if they approve it,  
**Keshav Singh:** If they are if they are objecting  
   
 

### 01:06:37

   
**Rachik Sharma:** if they are objecting,  
**Keshav Singh:** then  
**Rachik Sharma:** they will reject it. No, so they'll reject the packet.  
**Keshav Singh:** okay  
**Rachik Sharma:** If they reject the packet, nothing happens. ID ID remains activated as it uh no flag is added.  
**Keshav Singh:** it is huh mhm  
**Rachik Sharma:** Nothing is added. Okay. Uh so this is so and then approve or rejected you are done with the request. Okay. If after deactivation which is the second scenario if after deactivation another request for reactivation comes in. Okay. And also when we are writing the story for this scen for this scenario. Okay. This is this will clearly say why we are allowing the deactivation because it's a child record. We trust CRVS and anyway we are giving the packet to the manual verifier. manual manual verifier of the country will be an authority who will be designed designated for this role and we already have a stage in MOSIP for manual verification.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So considering all of this uh the packet is actually getting de deactivated after an approval from the national ID department.  
   
 

### 01:07:38

   
**Rachik Sharma:** So we are okay to deactivate and it is specific to the fraudulent or incorrect birth requests. Okay. Now let somebody now again comes to CRVS and says no no no the birth that you  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** the birth that you nullified is actually an right birth and we want the ID to be reactivated which is a irrational scenario. So the approach that we will take is we will not we will recommend is there is no support for reactivation.  
**Keshav Singh:** s\*\*\*.  
**Rachik Sharma:** There is no concept of reactivation within MOSIP or a child. Okay. Because if let's say a child ID is deactivated now let's say somebody goes to court does a fight okay there's let's say there is a fight between two parents and one of the parents to you know just to extract revenge has deactivated the ID of the child so that no child benefits are given and the parent starts you know getting the uh problems of finance and all that they go to court they fight it over they get the court order they go to CRVS saying see this was re deactivated reactivated Okay.  
   
 

### 01:08:43

   
**Rachik Sharma:** So from national ID perspective for us it doesn't make sense because the ID is already deactivated. What you do is you submit a new request and get a new ID.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Okay that's that that will be our recommendation. There is no concept of reactivation within MOSEP or within foundational ID from from  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** foundational ID perspective deactivation of a deactivated child ID does not make sense. uh so I mean does not make sense go we'll have to write in better words but there's basically we have to say there is no concept like this uh  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** because uh we cannot one of the reason is we cannot just simply open it up for the people to just toggle it on and off okay ID is a crucial thing somebody  
**Keshav Singh:** Correct. Correct. Uh great.  
**Rachik Sharma:** cannot  
**Keshav Singh:** It is crucial thing and practices would not be recommending. There must be some standard which would be guiding this the foundational ID system and its practices also there must be uh isn't it Rajik so in that in those guidelines also  
   
 

### 01:09:42

   
**Rachik Sharma:** Yes. Yes. Yes.  
**Keshav Singh:** it would be that you cannot play it like a general application that you are toggling on switch on and switch off.  
**Rachik Sharma:** Huh. Huh. So that's one of the reason. And second reason is why do you want to reactivate the same ID? You can uh get a new ID. You submit a new request and get a new ID. So that is for reactivation.  
**Keshav Singh:** Yes.  
**Rachik Sharma:** Similarly for death in this case it is valid that if somebody wants to reverse the reverse the death flag.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Okay to reverse the death flag somebody let's say it's a it's the same scenario. Somebody who was fighting on the battlefront comes back after one year and uh gets to know that their family thought that they are dead and they have uh deactivated the  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** ID or mark them as deceased. There is no concept of deactivation for adult.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** So in this since there is no concept of deactivation or adult there is no concept of reactivation also.  
   
 

### 01:10:43

   
**Rachik Sharma:** Okay. So all all we can do is we can update the flag. So that flag update is supported here. It will follow the same process. The story remains the same.  
**Keshav Singh:** All  
**Rachik Sharma:** We do not want to deactivate because there are a lot of interconnected services. So there is only a flag that is added so that service portals can know during authentication that the person is deceased. Similarly, we do since there is no deactivation, there is no concept of reactivation.  
**Keshav Singh:** right.  
**Rachik Sharma:** So for adults, somebody who is coming back and saying they're alive, we are okay to reverse the flag only. Okay. So for reversing the flag, same thing. We already had a flag which was used for uh marking the person as disease. We'll use the same flag and pass the value n. Okay. Okay. And once this value n is passed, we'll check that particular u against the earlier marked flag because in the records it  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** will have y.  
   
 

### 01:11:40

   
**Rachik Sharma:** That way we'll get to know that it's a death reversal packet. Okay. And also we'll have this process and there is no window for this because it's really hard to say uh how much time a person will take to come back and say he's not dead. Okay. There have been scenarios where people take five 10 years to come back and say he was actually he was alive  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** somewhere. Okay. Duplicates request will be rejected. Okay. And then again it is sent to manual verification. Manual verification will do their due diligence. If they approve it then the flag is reversed. If they reject it flag is not reversed. Okay. These three scenarios are also to come later. Okay. Now one thing that can be that has to be discussed further is the name of all these flags.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Okay. And to clearly say which stages the packet will go through. Mostly it will not be many stages because we want to send it to manual verification anyway.  
   
 

### 01:12:40

   
**Rachik Sharma:** there is no way actually we can verify whether the request is correct or not until we do it manually. So there not will not be many stages but still uh it's a it's something that uh we can have one or two like a basic dduplication saying that uh the request that is come in is not a duplicate request something like that. Okay. And so all of this can be decided but still story-wise in uh theoretical way we can still place these scenarios rather than focusing on technical part how to do it. But still why we are saying that we'll deactivate why we are saying we are not reactivating and why we are okay with reversal of death flag in what scenario scenarios this can take place those different things still we can add as you know uh scope in the document under here basically in this page we can have layer scenario section then we can decide what are the rare scenarios that we can explain why are the scenarios are like  
**Keshav Singh:** What is  
**Rachik Sharma:** that why are we saying uh that it's allowed to deactivate for child but not reactivate why there is no concept of reactivating what is our recommendation so that kind of documentation still we  
**Keshav Singh:** that?  
**Rachik Sharma:** can do technical part I forgot to actually I missed to clarify this because technical part we can still not put because some some things are not still not clear not finalized flow is finalized but tech design part is pending okay so this is the whole scope scope of this open CRVS and basically  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** CRVS and MOSIP. Okay.  
**Keshav Singh:** Mhm.  
**Rachik Sharma:** Uh any questions of  
**Keshav Singh:** Questions I was asking in between only and uh we'll go through the docs once. Hello.  
**Rachik Sharma:** Huh? Okay. Fine. Fine. Kisha.  
**Keshav Singh:** Hello.  
**Rachik Sharma:** Okay. Okay.  
**Keshav Singh:** Right.  
**Rachik Sharma:** Okay.  
**Keshav Singh:** I'm getting another call. Uh Rashik, I'll come back to you uh to this.  
**Rachik Sharma:** Stopping the  
   
 

### Transcription ended after 01:14:48

*This editable transcript was computer generated and might contain errors. People can also change the text after it was created.*