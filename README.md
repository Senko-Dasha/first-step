select   max_teaching_level
        , count(*) as cnt
from    (select t_1.*
        from      (select  user_id
                        , max_teaching_level
                        , class_start_datetime
                        , row_number () over (partition by user_id order by class_start_datetime desc) as last_classes
                  from skyeng_db.classes a 
                        left join skyeng_db.teachers b
                              on a.id_teacher = b.id_teacher
                    -- where class_status = 'success'
                     order by user_id) as t_1
        where last_classes <= 3) as t_2
group by max_teaching_level
