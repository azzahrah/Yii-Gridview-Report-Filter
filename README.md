Yii-Gridview-Report-Filter
==========================

[TODO: MAKE THE GRIDVIEW FILTER WIDGET!]

This extension will add new filter toolbar to your exisiting gridview.

[TODO: Add Demo Image Here]

How To Use 
----------

### Put this in your controller action

```php
public function actionReport() {

  Yii::import('ext.reportfilter.RFilterBuilder');
  
  #### Using CActiveDataProvider ####
  
  //You can add your own criteria condition or anything to do with the criteria here
  $criteria = new CDbCriteria;
  $criteria->compare('name','rizky');
  
  //or if you prefer array based criteria, it will do as well, 
  //but remember to treat your criteria as an array in your filter's preProcess function
  $criteria = array(
    'condition' => 'name = "rizky"'
  );
  
  $filter = new RFilterBuilder;
  
  //Filter your current criteria, 
  //the criteria will be passed to each postProcess function in RfilterWidget filter 
  $criteria = $filter->preProcess($criteria);
  
  $dataProvider = new CActiveDataProvider('YourModel', array('criteria'=>$criteria));

  #### Using CArrayDataProvider ####
  
  $anotherFilter = new RFilterBuilder;
  $anotherCriteria = new CDbCriteria;
  
  //you can preProcess your criteria here 
  $anotherCriteria = $anotherFilter->preProcess($criteria);
  
  $data = YourModel::model()->findAll($anotherCriteria);
  
  //you can postProcess your data before passing it to CArrayDataProvider
  //the data will be passed to each postProcess function in RfilterWidget filter 
  $data = $anotherFilter->postProcess($data);
  
  $arrayDataProvider = new CArrayDataProvider($data);

  //render our action
  $this->render('report', array(
    'filter' => $filter,
    'dataProvider' => $dataProvider,
    'anotherFilter' => $anotherFilter,
    'arrayDataProvider' => $arrayDataProvider
  ));

}
```

### Put this in your view
```php
<?php 
  $this->widget('ext.reportfilter.RFilterWidget', array(
    'id' => 'YourModelFilter'
    'gridviewID' => 'YourModelGridView',
    'filters' => array(
      array(
        'name' => 'date',
        'type' => 'daterange',
        'default' => array(
          'from' => now(),
          'to' => strtotime('+10 Day',now()),
        ),
        'preProcess' => function($criteria) {
          //you can filter your criteria here,
          //remember, criteria can be a CDbCriteria object or an Array
          
          //after you manipulate your criteria you must RETURN IT
          return $critera;
        },
        //you can also pass static function here, so it won't clutter your widget
        'postProcess' => ReportController::DateRangePostProcessor
      )
    
    )
    
  ));
?>
```


