Yii-Gridview-Report-Filter
==========================

This extension will add new filter toolbar to your exisiting gridview.

[TODO: Add Demo Image Here]


How To Use 
----------

* Put this in your controller action

```php

public function actionReport() {

  Yii::import('ext.reportfilter.RFilterBuilder');
  $filter = new RFilterBuilder;
  $criteria = $filter->criteria;
  
  //You can add your own criteria condition or anything to do with the criteria here
  $criteria->compare('name','rizky');
  $dataProvider = new CActiveDataProvider('YourModel', $criteria);

  //if you use array data provider, you can process your data further.
  //the data provider will be passed to FilterWidget post process function
  
  $anotherFilter = new RFilterBuilder;
  
  $data = YourModel::model()->findAll();
  $arrayDataProvider = new CArrayDataProvider($data);
  $arrayDataProvider = $anotherFilter->postProcess($arrayDataProvider);

  $this->render('report', array(
    'filter' => $filter,
    'dataProvider' => $dataProvider,
    'anotherFilter' => $anotherFilter,
    'arrayDataProvider' => $arrayDataProvider
  ));

}


```




