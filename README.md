#!/usr/bin/env python3
import requests
import json

# 查询渠道列表
response = requests.get('http://localhost:3000/api/channel/', 
                       headers={'Authorization': 'Bearer your-token'})
channels = response.json()

# 检查每个渠道
for channel in channels['data']:
    # 查询余额（具体API根据项目文档）
    balance = check_balance(channel['id'])
    
    if balance < 10:  # 余额低于10元
        # 禁用渠道
        requests.put(f'http://localhost:3000/api/channel/{channel["id"]}',
                    json={'status': 0},
                    headers={'Authorization': 'Bearer your-token'})
        print(f"禁用渠道: {channel['name']}, 余额: {balance}")
