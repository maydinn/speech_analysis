# project_spreech_analysis

DESCRIPTION:

In this repository, you will find the analyzes of speeches of some Turkish politicians
Also, you will find models to predict Which speech/sentence belongs to which politician


DATA:

Recep Tayyip Erdogan speeches : https://www.tccb.gov.tr/receptayyiperdogan/konusmalar/?&page=1 

Kemal Kilicdaroglu speeches: https://www.chp.org.tr/arama/5ad84d4587047e133396a14b

Devlet Bahceli speeches: http://www.mhp.org.tr/htmldocs/genel_baskan/konusmalari/mhp/Devlet_Bahceli_Konusmalari.html


examples of some important used methods:


#to navigate webpage by parameters of the link

for i in range(1, 23):
    path_page = 'https://www.tccb.gov.tr/receptayyiperdogan/konusmalar/?&page='+str(i)
    driver.get(path_page) 


#to get the text from the website by using class name

    element.click()
    text = WebDriverWait(driver, 30).until(
    EC.presence_of_element_located((By.CLASS_NAME, 'article-content'))
    )
    c = text.text
    driver.back()
    time.sleep(5)
    k_liste.append(c)


#to click each speech by using a dynamic xpath

    for j in range(1,len(elements)+1):
        path = '/html/body/div[2]/table/tbody/tr/td[2]/table/tbody/tr['+str(j)+']/td[2]/a'
        click = WebDriverWait(driver, 30).until(
        C.presence_of_element_located((By.XPATH, path))
        )
        click.click()

#an easy way to visualize bar plot

    result.label.value_counts().plot(kind='bar')

#getting all sentences as list
    ki=[]
    for i in df_k.parag:
        a = i.split('.')
        for j in a[4:]:
            ki.append(j)
#making label list for sentences
    label_k = ('kilicdaroglu '*len(ki)).split()
#making dataframe by using zips
    df_k_s=pd.DataFrame(zip(ki, label_k),columns=['sentece','label'])

Some insights:

1) In order to access data I used Selenium

2) There are some unbalances among leaders, the storage type can be a reason, because for some leaders all speeches save under different titles, for others save under one title.  

3) For each speech there are many sentences as salutations. That's why I preferred to drop them. Moreover, 'bir', 'de', etc, are used too much by the politicians. But in my opinion, these words should be dropped as well since it does not give insight deeply.

4) Since there are many columns, it is not easy to apply some models. In order to deal with this problem, PCA used to reduce the number of columns, but it was not capable to handle this large amount of columns. Instead, the TruncatedSVD was used.

    4.1) After using TruncatedSVD, the accuracy rate for logistic regression decreased, whereas the F1 and accuracy for XGBClassifier increased

    4.2) After reducing, we were able to use Randomforest, but the result was not better than logistic regression  

5) After dropping some intro sentences and some repeated words, the distribution of words changed, and it may give some important insights.

6) As a result;
    6.1) The prediction for the whole speech is really impressive, but it is reduced for sentences. 
    6.2) For the whole speech, the logistic regression worked very well, it predicted without any failure
    6.3) For the sentence level, several models  are applied, but overfitting and performance were major problems
    6.4) So that, the logistic regression predicted also better than other models for sentence prediction. as well.
7) Since there is a probability ratio for each label, it might give some insights about their relations as well. Results are quality similar for sentences and whole speeches dataframes. they indicate that there is a strong negative relationship between speeches of Erdogan and Bahceli while it is negative less weak for Kilicdaroglu. Euclidean Distance also shows that the distance between speeches of Erdogan and Bahceli is higher than ones between Erdogan and Kilicdaroglu, and Bahcel and Kilicdaroglu. Since Kilicdaroglu and Bahceli are opposition politicians, the higher similarity is reasonable. Moreover, having fewer observers for Kilicdaroglu can be also a reason to get a more similarity ratio for him. 

Limitation & Further:
1) Some models were not applied because of the limitation of my pc
2) Different parameter did not try for some models because of the limitation of my pc
3) With not only more but also balanced data, and deployment of different models better results can be obtained for sentence prediction. 
4) Dropping some unnecessary and repeated words might also provide better results






