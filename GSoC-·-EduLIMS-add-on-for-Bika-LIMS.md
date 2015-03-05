You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [GSoC 2015](https://github.com/bikalabs/Bika-LIMS/wiki/GSoC-2015) · EduLIMS add-on for Bika LIMS
***

### Summary

A Bika add-on specifically for educational centers, private or public, such as high schools, colleges, universities and technology centers.
EduLIMS focuses on activity tracking and evaluation of trainees, in the use LIMS itself and at the same time, learn the basics of a lab workflow, including procedures, QA/QC, instrument management, etc. 

With this in mind, the following are the modules to be implemented:

- Training environment: training sessions and guided training use-cases.
- Trainees activity tracking and evaluation
- Scoring module

**Expected results**: An EduLIMS add-on as described above

**Skills required**: [Python](http://python.org), [Plone](http://plone.org), [jQuery](http://www.jquery.com), CSS. Familiarity with Bika LIMS is a plus.

**Skill level**: High

**Mentors**: [J. Puiggené](http://github.com/xispa), [C. McKellar-Basset](http://github.com/rockfruit)
***

### Functional overview

Bika LIMS has strong security policies that apply depending on the user's role: Lab Manager, 
Analyst, Lab Clerk, Sampler, Preserver, Regulatory Inspector and Client. These are common roles 
used in any lab and each of them play its own rules in the Bika LIMS workflow. As an example, 
only Lab Managers are able to verify and publish analysis results, while Regulatory Inspectors can 
access to all electronic records but without the possibility to edit them. Clients can see their own 
Analysis Requests and manage their own contacts but can not see most of the steps of the analysis 
workflow.

While these roles allow to fullfil most of any lab workflow requirements, don't fit well with a 
training center requirements. This is mainly because in an educational environment, the main 
purpose is the students to learn how the LIMS and lab Quality Assessment and Quality Control 
works, as well as perform a pre­defined use cases to learn how a typical lab workflow is. This 
means that some of the system built­in rules compliant with ISO 17025 prevents students to perform 
actions we consider important for their training.

We suggest to add the 'Trainer' and 'Trainee' roles. 

**a) Training session**

A training session will allow the trainnees practice with the system without the *productive* side of the LIMS being altered. Training sessions might have the following properties:

- A training session can only be created by a Trainer.
- Trainee can view and modify only the records from within a training session
- Trainees can view and participate only on those training sessions to which they are registered. The assignment of trainees to a training session is performed by the trainer.
- Out of a training session context, only the 'generic' electronic records (those not assigned to any training session) are displayed. An electronic record (Analysis Request, Worksheet, Analysis Specification, Calculation, etc.) created inside a training session will not be displayed out of the training session context.
- The trainer can assign generic records not linked to any session to be available in a specific training session. The modification of these records in a training session will not cause any effect to the 'generic' ones.
- A trainer can close a training session. Closed training sessions cannot be accessed by any of the assigned trainees.

**b) Traineers activity tracking and reporting**

Every action done by a traineer in a training session is recorded for further evaluation. Trainer can track at runtime the activity of any participant from a training session. Also, trainees can generate an activity report for the whole training session.

**c) Training use­-case**

A training use­-case is a custom use­-case workflow defined manually by the trainer to be applied in a training session. By creating a training use­-case, the trainer sets the rules to which the training session's objectives are based on. The trainees should then accomplish these rules and objectives in a training session. As an example, the trainer could define the following training use-­case:

1. Create an Analysis Request for the Client 'Happy Hills' with 2 samples of type 'Waste water', assign the analyses *E.coli* and *Clostridium* and set 'Elizabeth Mary' as the default contact.

2. Receive the Analysis Request created previously

3. Preserve the sample partitions from the previous Analysis Request

4. Add the analysis *Enterococcus* to the Analysis Request previously created

5. ...

The system could track trainee activities and find matches with the workflow above. This could be used later to evaluate if a trainnee has succeed with the planned objectives and score his/her activity in a training session.

**d) Scoring module**

The scoring subsystem allows the trainer to evaluate the activity done by a trainee within a training 
session by using the information recorded regards to the training use­-cases.

