#Задание один
код по которому выполнил

import json

def read_purchase_log(file_path):
    purchases = {}
    with open(file_path, 'r', encoding='utf-8') as f:
        next(f)
        for line in f:
            line = line.strip()
            if not line:
                continue
            data = json.loads(line)
            purchases[data['user_id']] = data['category']
    return purchases

def process_visit_log(input_file, output_file, purchases):
    with open(input_file, 'r', encoding='utf-8') as f_in, open(output_file, 'w', encoding='utf-8') as f_out:
        header = next(f_in).strip().split(',')
        header.append('category')
        f_out.write(','.join(header) + '\n')
        
        for line in f_in:
            line = line.strip().split(',')
            user_id = line[0]
            if user_id in purchases:
                line.append(purchases[user_id])
                f_out.write(','.join(line) + '\n')

purchase_log_file = r'Downloads\purchase_log.txt'
visit_log_file = r'Downloads\visit_log.csv'
funnel_file = 'funnel.csv'

purchases = read_purchase_log(purchase_log_file)

process_visit_log(visit_log_file, funnel_file, purchases)

print("Готово. Файл funnel.csv успешно создан.")

