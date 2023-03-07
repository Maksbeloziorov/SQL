------analyse REDMAIN(opensource)--------------------------------------------------------------------------------------
select distinct journalized_id                                                  as Зад,
                t.name                                                          as Трек,
                i_i.subject                                                     as Тема,
                i_s.name                                                        as Стат,
                ens.name                                                        as Прио,
                sum(hours) over (partition by i_i.id)                           as Время,
                i_i.created_on                                                  as Созд,
                i_i.updated_on                                                  as Обнов,
                i_i.closed_on                                                   as Закр,
                round(extract(epoch from j_j.created_on - i_i.created_on) / 60) as Создано_Решено,
------------------------------------------------------------------------------------------------------------------
                (select sum(inter.timest)
                 from (select round(extract(epoch from jj.created_on - lag(jjj.created_on) over (order by j.id)) /
                                    60) as timest
                       from journals j
------------------добавляем колонку с отметкой " гот " и " соз "----------------------------------------
                                left outer join journals jj on jj.id = j.id and jj.id in (
------------------------------------------------------------------------------------------------------------------
                           select jj.id
                           from journals jj
                                    join issues i on jj.journalized_id = i.id
                                    join journal_details jd on jj.id = jd.journal_id
                                    join users u on jj.user_id = u.id
                           where jj.journalized_id = j_j.journalized_id
--                              and jd.prop_key is not null
                             and jd.prop_key not in ('assigned_to_id', 'done_ratio')
                             and value not in ('5')
                             and jj.private_notes = 'false'
                             and old_value != '1'
                             and jj.id not in (select max(j.id)
                                               from journals j
                                                        left join journal_details jd on j.id = jd.journal_id
                                               where journalized_id = j_j.journalized_id
                                                 and prop_key is not null
                                                 and prop_key not in ('assigned_to_id', 'done_ratio')
                                                 and value not in ('5')
                                                 and private_notes = 'false'
                                               group by j.journalized_id)
                             and old_value in ('3', '4')
                           order by jj.id
                       )
----------------добавляем колонку с отметкой " раб "--------------------------------------------
                                left outer join journals jjj on jjj.id = j.id and jjj.id in (
---------------------------------------------------------------------------------------------------
                           select jj.id
                           from journals jj
                                    join issues i on jj.journalized_id = i.id
                                    join journal_details jd on jj.id = jd.journal_id
                                    join users u on jj.user_id = u.id
                           where jj.journalized_id = j_j.journalized_id
--                              and jd.prop_key is not null
                             and jd.prop_key not in ('assigned_to_id', 'done_ratio')
                             and value not in ('5')
                             and jj.private_notes = 'false'
                             and old_value != '1'
                             and jj.id not in (select max(j.id)
                                               from journals j
                                                        left join journal_details jd on j.id = jd.journal_id
                                               where journalized_id = j_j.journalized_id
                                                 and prop_key is not null
                                                 and prop_key not in ('assigned_to_id', 'done_ratio')
                                                 and value not in ('5')
                                                 and private_notes = 'false'
                                               group by j.journalized_id)
                             and old_value = '2'
                           order by jj.id
                       )
------------------------------------------------------------
                                join issues i on j.journalized_id = i.id
                                join journal_details jd on j.id = jd.journal_id
                                join users u on j.user_id = u.id
                       where j.journalized_id = j_j.journalized_id
--                          and jd.prop_key is not null
                         and jd.prop_key not in ('assigned_to_id', 'done_ratio')
                         and jd.value not in ('5')
                         and j.private_notes = 'false'
                         and jd.old_value != '1'
                         and j.id not in (select max(j.id)
                                          from journals j
                                                   left join journal_details jd on j.id = jd.journal_id
                                          where journalized_id = j_j.journalized_id
                                            and prop_key is not null
                                            and prop_key not in ('assigned_to_id', 'done_ratio')
                                            and value not in ('5')
                                            and private_notes = 'false'
                                          group by j.journalized_id)
                       order by j.id) as inter)                                 as Интервал
-----------продолжение-----------------------------------------------------------
from issues i_i
         join journals j_j on j_j.journalized_id = i_i.id
         join trackers t on i_i.tracker_id = t.id
         join issue_statuses i_s on i_i.status_id = i_s.id
         join enumerations ens on i_i.priority_id = ens.id
         left join time_entries te on i_i.id = te.issue_id
where i_i.project_id = 25
  and j_j.id in (select max(j.id)
                 from journals j
                          left join journal_details jd on j.id = jd.journal_id
                 where prop_key is not null
                   and prop_key not in ('assigned_to_id', 'done_ratio')
                   and value in ('3')
                   and private_notes = 'false'
                 group by j.journalized_id)
------фильтр------------------------------------------------------------------------------------------
  and ((i_i.created_on < '2022-07-01' and closed_on is null) -- до *** и не закр
    or (i_i.created_on < '2022-07-01' and i_i.closed_on between '2022-07-01' and '2022-07-31') -- между ** и **
    or (i_i.created_on between '2022-07-01' and '2022-07-31')) -- между ** и **
------------------------------------------------------------------------------------------------
order by journalized_id;



------Продолжение-----------------------------------------------------------------------------------
select distinct journalized_id                                                  as Зад,
                t.name                                                          as Трек,
                i_i.subject                                                     as Тема,
                i_s.name                                                        as Стат,
                ens.name                                                        as Приор,
                sum(hours) over (partition by i_i.id)                           as Затр,
                i_i.created_on                                                  as Созд,
                i_i.updated_on                                                  as Обн,
                i_i.closed_on                                                   as Закр,
                round(extract(epoch from j_j.created_on - i_i.created_on) / 60) as Создано_Решено
from issues i_i
         join journals j_j on j_j.journalized_id = i_i.id
         join trackers t on i_i.tracker_id = t.id
         join issue_statuses i_s on i_i.status_id = i_s.id
         join enumerations ens on i_i.priority_id = ens.id
         left join time_entries te on i_i.id = te.issue_id
where i_i.project_id = 25
  and j_j.id in (select max(j.id)
                 from journals j
                          left join journal_details jd on j.id = jd.journal_id
                 where prop_key is not null
                   and prop_key not in ('assigned_to_id', 'done_ratio')
                   and value in ('5')
                   and private_notes = 'false'
                 group by j.journalized_id)
------фильтр------------------------------------------------------------------------------------------
 and ((i_i.created_on < '2022-07-01' and closed_on is null) --созд до *** и не закр
    or (i_i.created_on < '2022-07-01' and i_i.closed_on between '2022-07-01' and '2022-07-31') --закр между ** и **
    or (i_i.created_on between '2022-07-01' and '2022-07-31')) --созд между ** и **
and i_i.id not in(80884,82032)
------------------------------------------------------------------------------------------------
order by journalized_id;
