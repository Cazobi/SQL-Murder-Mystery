# SQL
 My SQL Code
SELECT * FROM crime_scene_report
WHERE type = 'murder' AND city = 'SQL City'
--Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave".
SELECT * FROM person
WHERE address_street_name = 'Franklin Ave' AND name LIKE 'Annabel%'
SELECT *, MAX(address_number) FROM person
WHERE address_street_name = 'Northwestern Dr'
--id	name	license_id	address_number	address_street_name	ssn
--16371	Annabel Miller	490173	103	Franklin Ave	318771143
--14887	Morty Schapiro	118009	4919	Northwestern Dr	111564949
SELECT * FROM interview
WHERE person_id = '16371' OR person_id = '14887'
--14887	I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W".
--16371	I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.
SELECT * FROM get_fit_now_member
WHERE membership_status = 'gold' AND id LIKE '48Z%'
--id	person_id	name	membership_start_date	membership_status
--48Z7A	28819	Joe Germuska	20160305	gold
--48Z55	67318	Jeremy Bowers	20160101	gold
SELECT * FROM get_fit_now_check_in
WHERE 
	check_in_date LIKE '%0109' 
AND
	membership_id = '48Z7A' OR membership_id = '48Z55'
--membership_id	check_in_date	check_in_time	check_out_time
--48Z7A	20180109	1600	1730
--48Z55	20180109	1530	1700
SELECT * FROM person
WHERE name = 'Joe Germuska' OR name = 'Jeremy Bowers'
--id	name	license_id	address_number	address_street_name	ssn
--28819	Joe Germuska	173289	111	Fisk Rd	138909730
--67318	Jeremy Bowers	423327	530	Washington Pl, Apt 3A	871539279
SELECT * FROM drivers_license
WHERE plate_number LIKE '%H42W%'
-- Culprit 423327	30	70	brown	brown	male	0H42W2	Chevrolet	Spark LS

--REAL CULPRIT--

SELECT * FROM interview
WHERE person_id = '67318'
--I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.
SELECT *, COUNT(person_id) FROM facebook_event_checkin
WHERE date LIKE '201712%' AND event_name = 'SQL Symphony Concert'
GROUP BY person_id
ORDER BY COUNT(person_id) DESC
--person_id	event_id	event_name	date	COUNT(person_id)
--24556	1143	SQL Symphony Concert	20171224	3
--99716	1143	SQL Symphony Concert	20171229	3
SELECT * FROM drivers_license
WHERE car_make = 'Tesla' AND car_model = 'Model S'
AND hair_color = 'red' AND gender = 'female'
--id	age	height	eye_color	hair_color	gender	plate_number	car_make	car_model
--202298	68	66	green	red	female	500123	Tesla	Model S
--291182	65	66	blue	red	female	08CM64	Tesla	Model S
--918773	48	65	black	red	female	917UU3	Tesla	Model S
SELECT * FROM person
WHERE (license_id = '202298' OR license_id = '291182' OR license_id = '918773')
AND id = '24556' OR id = '99716'
--id	name	license_id	address_number	address_street_name	ssn
--99716	Miranda Priestly	202298	1883	Golden Ave	987756388

--TRUE ANSWER--

--Miranda Priestly
