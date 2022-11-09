---
layout: post
title: '[Python] csv 및 npy 파일 소수점 단위 동일성 검정'
category: Python
tags: [python]
comments: true
---

# Python csv 및 npy 파일 소수점 단위 동일성 검정

~~~python
#-*- coding:utf-8 -*-

import pandas as pd
import numpy as np
import argparse
from datetime import datetime

def logging(before_data, after_data, start_time, type, before_df=None, after_df=None):
    end_time = datetime.now()
	print("before data >>> {}".format(before_data), flush = True)
    print("after data >>> {}".format(after_data), flush = True)
	
    if type == True:
	    print("before count: {} vs after count {}".format(len(before_df), len(after_df)), flush = True)
        print("The data are the same!", flush = True)
        result.append(True)
    elif type == False:
		print("before count: {} vs after count {}".format(len(before_df), len(after_df)), flush = True)
        print("The data are NOT the same!", flush = True)
        result.append(False)
	elif type == "Error1":
		print("[ERROR] data comparison failed!", flush = True)
		result.append(False)
	elif type == "Error2":
	    print("[ERROR] please check the data type is csv or npy!", flush = True)
        result.append(False)

	print("duration time is {} sec".format((end_time - start_time).seconds), flush = True)
    print("-"*50, flush = True)

def main(before_data_list_file, after_data_list_file, decimal_point):
    global result
    result = []
    start_time = datetime.now()
    print("="*50, flush = True)
    print("before vs after data comparison start!", flush = True)
    print("start time is {}".format(start_time))
    print("="*50, flush = True)
    
    # before, after 프로그램 목록을 리스트에 각각 저장한다.
    before_data_list = [before.rstrip('\n') for before in open(before_data_list_file)]
    after_data_list = [after.rstrip('\n') for after in open(after_data_list_file)]
    
    # 소수점 절사
    decimal_num = 10 ** decimal_point

    for before_data, after_data in zip(before_data_list, after_data_list):
        
		start_time_tmp = datetime.now()

        try:
            if before_data.split(".")[-1] == "npy" and after_data.split(".")[-1] == "npy":
                before_arr = np.load(before_data)
                after_arr = np.load(after_data)

                try:
                    before_arr = (before_arr * decimal_num // 1) / decimal_num
                    after_arr = (after_arr * decimal_num // 1) / decimal_num
                except:
                    print("The npy data type is not a numeric type.", flush = True)
                    print("before data type: {} vs after data type: {}".format(before_arr.dtype,
                                                                        after_arr.dtype), flush = True)     

                if np.array_equal(before_arr, after_arr):
                    logging(before_data, after_data, start_time_tmp, True, before_arr, after_arr)
                else:
                    logging(before_data, after_data, start_time_tmp, False, before_arr, after_arr)


            elif before_data.split(".")[-1] == "csv" and after_data.split(".")[-1] == "csv":

                # before, after 프로그램 로드
                before_df = pd.read_csv(before_data, engine="python")
                after_df = pd.read_csv(after_data, engine="python")

                before_numeric_columns = before_df.select_dtypes(include=np.number).columns.tolist()
                after_numeric_columns = after_df.select_dtypes(include=np.number).columns.tolist()

                before_df[before_numeric_columns] = \
                  before_df[before_numeric_columns].apply(lambda x: (x * decimal_num // 1) / decimal_num)

                after_df[after_numeric_columns] = \
                  after_df[after_numeric_columns].apply(lambda x: (x * decimal_num // 1) / decimal_num)

                # before_df.infer_objects()
                # before_df.dtypes[before_df.dtypes != "object"].index

                # before_df와 after_df의 동일성 비교
                if before_df.equals(after_df):
                    logging(before_data, after_data, start_time_tmp, True, before_df, after_df)
                else:
                    logging(before_data, after_data, start_time_tmp, False, before_df, after_df)

            else:
                logging(before_data, after_data, start_time_tmp, "Error2")
                continue
        except:
            logging(before_data, after_data, start_time_tmp, "Error1")
            continue
            
    end_time = datetime.now()
    print("="*50, flush = True)
    print("before vs after data comparison finish!", flush = True)
    if set(result) == {True}:
        print("all data is the same", flush = True)
    else:
        print("[WARNING] NOT all data is the same", flush = True)
    print("end time is {}".format(end_time))
    print("duration time is {} sec".format((end_time - start_time).seconds))
    print("="*50, flush = True)
        
        
if __name__ == "__main__":

    parser = argparse.ArgumentParser()

    parser.add_argument('before', type=str, help="before data list file")
    parser.add_argument('after', type=str, help="after data list file")
    parser.add_argument('decimal', type=int, default=15, help="decimal point")
    
    args = parser.parse_args()
    
    _before_data_list_file = args.before
    _after_data_list_file = args.after
    _decimal_point = args.decimal
    
    main(_before_data_list_file, _after_data_list_file, _decimal_point)
~~~

## 실행
python   ./compare.py   <파일리스트>   <비교할파일리스트>  <검증할소수점자리수>

~~~bash
$ python   ./compare.py   com1.txt   com2.txt   20
~~~