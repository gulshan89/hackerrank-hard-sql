with cte1 as
(
select a.hacker_id,a.name,a.contest_id,
isnull(sum(total_views),0) as total_views,
isnull(sum(total_unique_views),0) as total_unique_views from 
contests a inner join colleges b on a.contest_id=b.contest_id
inner join challenges c on c.college_id=b.college_id
left join view_Stats d on d.challenge_id=c.challenge_id
group by a.hacker_id,a.name,a.contest_id
)
,
cte2 as
(
select a.hacker_id,a.name,a.contest_id,
isnull(sum(total_submissions),0) as total_submissions,
isnull(sum(total_accepted_submissions),0) as total_accepted_submissions from 
contests a inner join colleges b on a.contest_id=b.contest_id
inner join challenges c on c.college_id=b.college_id
left join Submission_Stats d on d.challenge_id=c.challenge_id
group by a.hacker_id,a.name,a.contest_id
)

select b.contest_id,a.hacker_id,a.name,b.total_submissions,
b.total_accepted_submissions,a.total_views,a.total_unique_views
from cte1 as a inner join cte2 as b on a.contest_id=b.contest_id
where b.total_submissions+b.total_accepted_submissions+
a.total_views+a.total_unique_views > 0
order by contest_id
