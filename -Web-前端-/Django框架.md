【数据库交互】：
python manage.py shell
from polls.models import Choice,Question

Question.objects.all()

from django.utils import timezone
q = Question(quesetion_text='What is New?'pub_date=timezone.now())

q.save()
q.id
q.quesetion_text

Question.objects.all()

ctrl +z(退出)
(重新进入)
from polls.models import Choice.Question
Question.objects.all()
Question.objects.filter()


from django.utils import timezone
current_year = timezone.now().year
current_year        (回车)

q = Question.objects.get(pk=1)
q（enter）

q.chocie_set.all()

q.chocie_set.create(choice_text='Not much..',votes=0)

q.chocie_set.create(choice_text='The sky.',votes=0)

q.chocie_set.create(choice_text='Just hacking again',votes=0)


q.choice_set

q.choice_set.all()

q.choice_set.count()

c = q.choice_set.filter(choice_text_startwith="Jus")
c
c.delete()

---

python manage.py createuperuser
admin
1178824808@qq.com
admin













