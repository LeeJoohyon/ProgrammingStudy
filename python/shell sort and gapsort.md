#shell 정렬
셸 정렬 (때로는 "감소분 증가 정렬"이라고 함)은 원본 정렬을 삽입 정렬을 사용하여 정렬 된 여러 개의 작은 하위 목록으로 분리하여 삽입 정렬을 향상시킵니다. 이러한 하위 목록을 선택하는 고유 한 방법은 셸 정렬의 핵심입니다. 인접한 항목의 하위 목록으로 목록을 분할하는 대신 쉘 정렬은 i 항목을 구분하여 하위 목록을 만들기 위해 간격 (gap)이라고도하는 증가 i를 사용합니다.

그림 6에서 볼 수 있습니다.이 목록에는 9 개의 항목이 있습니다. 증가분을 세 개 사용하면 세 개의 하위 목록이 있으며 각 하위 목록은 삽입 정렬로 정렬 할 수 있습니다. 이러한 정렬을 완료하면 그림 7과 같은 목록이 표시됩니다.이 목록이 완전히 정렬되지는 않았지만 매우 흥미로운 것이있었습니다. 하위 목록을 정렬하여 항목을 실제로 속한 곳으로 이동 시켰습니다.

![shell sort](http://interactivepython.org/courselib/static/pythonds/_images/shellsortA.png)
![shell sort2](http://interactivepython.org/courselib/static/pythonds/_images/shellsortB.png)

기본적인 방법 len(list)의 길이를 반으로 나눠 첫번째와 반으로나눈길이의 숫자에있는 argument와 비교하여 반복한다.

#merge sort 