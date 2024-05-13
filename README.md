# SQL Murder Mystery

My solution to the Murder Mystery Challenge posted by Knight Lab https://mystery.knightlab.com/
 
SOLUTION SCRIPT

The murder occured sometime on Jan. 15, 2018

	1. FIND INFORMATION ABOUT THE CRIME
   
SELECT * FROM crime_scene_report
WHERE type = 'murder' AND city = 'SQL City'

Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave".

	2. FIND INFORMATION ABOUT THE WITNESSES
   
SELECT * FROM person
WHERE address_street_name = 'Franklin Ave' AND name LIKE 'Annabel%'
SELECT *, MAX(address_number) FROM person
WHERE address_street_name = 'Northwestern Dr'

Witness 1:
16371	Annabel Miller	490173	103	Franklin Ave	318771143

Witness 2:
14887	Morty Schapiro	118009	4919	Northwestern Dr	111564949


	3. LOOK UP WITNESSES STATEMENTS
   
SELECT * FROM interview
WHERE person_id = '16371' OR person_id = '14887'

Witness 1:
I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.

Witness 2:
I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W".

	4. FIND KILLER USING WITNESS INFORMATION

SELECT * FROM get_fit_now_member
WHERE membership_status = 'gold' AND id LIKE '48Z%'

SELECT * FROM get_fit_now_check_in
WHERE  check_in_date LIKE '%0109' 
AND  membership_id = '48Z7A' OR membership_id = '48Z55'

Suspect 1:

48Z7A	28819	Joe Germuska	20160305	gold

48Z7A	20180109	1600	1730

Suspect 2:

48Z55	67318	Jeremy Bowers	20160101	gold

48Z55	20180109	1530	1700


SELECT * FROM person
WHERE name = 'Joe Germuska' OR name = 'Jeremy Bowers'

28819	Joe Germuska	173289	111	Fisk Rd	138909730

67318	Jeremy Bowers	423327	530	Washington Pl, Apt 3A	871539279

SELECT * FROM drivers_license
WHERE plate_number LIKE '%H42W%'

CULPRIT

Culprit 423327	30	70	brown	brown	male	0H42W2	Chevrolet	Spark LS

	FIND THE MASTERMIND

SELECT * FROM interview
WHERE person_id = '67318'

I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.

SELECT *, COUNT(person_id) FROM facebook_event_checkin
WHERE date LIKE '201712%' AND event_name = 'SQL Symphony Concert'
GROUP BY person_id
ORDER BY COUNT(person_id) DESC

24556	1143	SQL Symphony Concert	20171224	3

99716	1143	SQL Symphony Concert	20171229	3

SELECT * FROM drivers_license
WHERE car_make = 'Tesla' AND car_model = 'Model S'
AND hair_color = 'red' AND gender = 'female'

202298	68	66	green	red	female	500123	Tesla	Model S

291182	65	66	blue	red	female	08CM64	Tesla	Model S

918773	48	65	black	red	female	917UU3	Tesla	Model S

SELECT * FROM person
WHERE (license_id = '202298' OR license_id = '291182' OR license_id = '918773')
AND id = '24556' OR id = '99716'

99716	Miranda Priestly	202298	1883	Golden Ave	987756388

MASTERMIND: Miranda Priestly
