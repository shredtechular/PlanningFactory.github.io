
####Basic Join of DataFactory Tables
Delete the joins you dont need to increase speed and keep statement simple

    SELECT 
       fV.FactoryID
      ,fV.ProductLineID
      ,fV.ProductID
      ,fV.ValueSeriesID
      ,CAST(fV.ValueInt AS money)/dVS.Scale AS Value
      ,fV.ValueInt

    FROM 
      -- Basic fact table (ValueInt, ValueText, ValueComment)
      sx_pf_fValues fV
    
      -- for Factory Attributes (Name, RespPerson...)
      LEFT JOIN sx_pf_dFactories dF ON 
        fV.FactoryKey = dF.FactoryKey
      
      -- for Productline Attributes (Name, RespPerson..)
      LEFT JOIN sx_pf_dProductLines dPL ON
        fV.ProductLineKey = dPL.ProductLineKey

      -- for Product Attributes (Name, Status, Globalattributes 1..25)
      LEFT JOIN sx_pf_dProducts dP ON
        fV.ProductKey = dP.ProductKey

      -- for ValueSeries Attributes (Effect, VisibilityLevel, NumericFlag)
      LEFT JOIN sx_pf_dValueSeries dVS ON 
        fV.ValueSeriesKey = dVS.ValueSeriesKey

    WHERE
       dF.FactoryID != 'ZT'  -- Filter the Templates
