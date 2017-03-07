#Sorting and search
###Sequential search(순차검색)
모든 데이터를 하나하나 순서에 맞춰 하나하나 검색하는 알고리즘
![sequece search](http://interactivepython.org/courselib/static/pythonds/_images/seqsearch.png)

	def sequentialSearch(alist, item):
	    pos = 0
	    found = False
	
	    while pos < len(alist) and not found:
	        if alist[pos] == item:
	            found = True
	        else:
	            pos = pos+1
	
	    return found
	
	testlist = [1, 2, 32, 8, 17, 19, 42, 13, 0]
	print(sequentialSearch(testlist, 3))
	print(sequentialSearch(testlist, 13))
	
###binary search
리스트의 중간점을 정해서 중간점보다 입력된 값을 비교하여 값을 찾아낸다.
(정렬된 리스트에서 사용이 가능하다)
![binary search](http://interactivepython.org/courselib/static/pythonds/_images/binsearch.png)

	def binarySearch(alist, item):
	    first = 0
	    last = len(alist)-1
	    found = False
	
	    while first<=last and not found:
	        midpoint = (first + last)//2
	        if alist[midpoint] == item:
	            found = True
	        else:
	            if item < alist[midpoint]:
	                last = midpoint-1
	            else:
	                first = midpoint+1
	
	    return found
	    

###Bubble sort

버블 정렬은 목록을 통해 여러 패스를 만듭니다. 인접한 항목을 비교하고 순서가 잘못된 항목을 교환합니다. 목록을 통과 할 때마다 적절한 위치에 다음으로 큰 값이 배치됩니다. 본질적으로 각 항목은 해당 항목이 속한 위치까지 "버블 링"됩니다.
(리스트가 순차적으로 되어있을때 가장 효율적이다.)
O = n제곱
	
	def bubbleSort(x):
	length = len(x)-1
	for i in range(length):
		for j in range(length-i):
			if x[j] > x[j+1]:
				x[j], x[j+1] = x[j+1], x[j]
	return x	

![bubble sort](http://interactivepython.org/courselib/static/pythonds/_images/bubblepass.png)
###Selection sort
선택 정렬(選擇整列, selection sort)은 제자리 정렬 알고리즘의 하나로, 다음과 같은 순서로 이루어진다.

1. 주어진 리스트 중에 최솟값을 찾는다.
2. 그 값을 맨 앞에 위치한 값과 교체한다(패스(pass)).
3. 맨 처음 위치를 뺀 나머지 리스트를 같은 방법으로 교체한다.

비교하는 것이 상수 시간에 이루어진다는 가정 아래, n개의 주어진 리스트를 이와 같은 방법으로 정렬하는 데에는 Θ(n2) 만큼의 시간이 걸린다.

	def selectionSort(x):
	length = len(x)
	for i in range(length-1):
	    indexMin = i
		for j in range(i+1, length):
			if x[indexMin] > x[j]:
				indexMin = j
		x[i], x[indexMin] = x[indexMin], x[i]
	return x


![selection sort](http://interactivepython.org/courselib/static/pythonds/_images/selectionsortnew.png)

###Insertion sort	
삽입 정렬(揷入整列, insertion sort)은 자료 배열의 모든 요소를 앞에서부터 차례대로 이미 정렬된 배열 부분과 비교하여, 자신의 위치를 찾아 삽입함으로써 정렬을 완성하는 알고리즘이다.

k번째 반복 후의 결과 배열은, 앞쪽 k + 1 항목이 정렬된 상태이다.
	
	31,25,12,22,11 	처음 상태
	31,[25],12,22,11 	두 번째 원소를 부분 리스트에서 적절한 위치에 삽입한다.
																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																			<25>,31,[12],22,11  세 번째 원소를 부분 리스트에서 적절한 위치에 삽입한다.
	<12>,25,31,[22],11 	 네 번째 원소를 부분 리스트에서 적절한 위치에 삽입한다.
	12 ,<22>,25,31,[11] 	마지막 원소를 부분 리스트에서 적절한 위치에 삽입한다.
	<11>,12,22,25,31 	종료.
		
	def insertSort(x):
		for i in range(1, len(x)):
			j = i - 1
			key = x[i]
			while x[j] > key and j >= 0:
				x[j+1]  = x[j]
				j = j - 1
			x[j+1] = key
		return x



	    