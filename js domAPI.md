str.indexOf("����")���ҵ��ַ����кͲ���һ�����ַ��������ص��ǳ��ֵ��������Ҳ�������-1��

str.startsWith("����")���ж��ַ����Ƿ��Բ�����ͷ�����ص��ǲ������͡�
str.endsWith("����")���ж��ַ����Ƿ��Բ�����β�����ص��ǲ������͡�

H5����������DOM����

ѡ������
document.querySelector('����1') �������ȡ����class���. id���# ��ǩʲô������
document.querySelectorAll('����1')����ȡ��һ������

����class���ԣ�

.classList ����ȡ����Ԫ�ص�ȫ����ʽ�����飩
.add("class����") �����Ҫ���������ʽҪд���飬����document.querySelector('li').classList.add('rad')
document.querySelector('li').classList.add('blue')
.remove('class����')���Ƴ�class��ʽ�������Ƴ�class���ԣ�����document.querySelector('li').classList.remove('rad')
.toggle('class����'):���class��ʽû������ӣ�����ɾ��������document.querySelector('li').classList.toggle('rad')
.contains('class����'): �ж�Ԫ���Ƿ����ָ�����Ƶ���ʽ������true��false ����document.querySelector('li').classList.contains('rad')

local����
localStorage.setItem()
