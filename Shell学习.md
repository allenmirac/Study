```shell
#!/bin/bash

# 捕捉 ctrl+c 信号，执行 save_address_book 函数
trap trap_INT INT

declare -a address_book

# 获取每个关系的人数，并将其保存到一个关系数组中
declare -A relationNums
# 家庭
relationNums["1"]=0
# 工作
relationNums["2"]=0
# 同学
relationNums["3"]=0
# 亲友
relationNums["4"]=0

function trap_INT(){
    save_address_book
    exit 0
}

# 加载通讯录
function load_address_book() {
    # 如果文件不存在则直接返回, -f == "-e FILENAME && -f FILENAME", -e表示判断文件是否存在，-f表示判断是否是普通文件
    if [ ! -f "tongxun.txt" ]; then
        touch tongxun.txt
        return
    fi

    while read line
    do
        # 将读取到的行按照 | 分割成不同的字段，并添加到数组中
        address_book+=("$line")
    done < "tongxun.txt"
}

# 保存通讯录
function save_address_book() {
    # 将数组中的所有元素逐行写入文件中, %s\n
    printf "%s\n" "${address_book[@]}" > "tongxun.txt"
    echo "通讯录已保存"
}

# 备份通讯录
function backup_address_book() {
    # 备份通讯录数据到文件
    touch address_book_backup.txt
    printf "%s\n" "${address_book[@]}" > "address_book_backup.txt"

    # 加密备份文件
    openssl aes-256-cbc -salt -in address_book_backup.txt -out address_book_encrypted.bin

    # 删除明文备份文件
    rm address_book_backup.txt

    # # 解密备份文件
    # read -p "请输入解密密码：" password
    # openssl aes-256-cbc -d -salt -in address_book_encrypted.bin -out address_book_decrypted.txt -pass pass:$password

    echo "备份已完成。"
}

# 解密备份文件
function backup_address_book_decrypted(){
    read -p "请输入解密密码：" password
    openssl aes-256-cbc -d -salt -in address_book_encrypted.bin -out address_book_decrypted.txt -pass pass:$password

    rm address_book_encrypted.bin
    echo "备份已解密!!!^_^"
}

function add_address_book() {
    echo "请输入通讯录类型: "
    read type

    if [[ "$type" != "1" && "$type" != "2" && "$type" != "3" && "$type" != "4" ]]; then
        echo "类型错误: type不是1 2 3 4中的任何一个. "
        echo "请再输入一次: "
        add_address_book
        return 0
    fi
    # echo "通讯录类型:$type"
    relationNums["$type"]=0
    while IFS='|' read -r id name rel phone remark; do
        if [ "$rel" == "$type" ]; then
            # echo "$type"
            # add a relation
            relationNums["$type"]=$(( ${relationNums[$type]} + 1 ))
        fi
    done < tongxun.txt
}

function refresh_address_book_relationNums(){
    relation="$1"
    # # 家庭
    # relationNums["1"]=0
    # # 工作
    # relationNums["2"]=0
    # # 同学
    # relationNums["3"]=0
    # # 亲友
    # relationNums["4"]=0
    if [ ${relationNums["$relation"]} -eq 0 ]; then
        return 0;
    else
        relationNums["$relation"]=0
    fi
    while IFS='|' read -r id name rel phone remark; do
        if [ "$relation" == "$rel" ]; then
            relationNums["$rel"]=$(( ${relationNums[$rel]} + 1 ))
        fi
    done < tongxun.txt
}

function query_address_book() {
    echo "所有通讯录信息如下: "

    flag=0
    # 输出每个关系的人数
    for relation in "${!relationNums[@]}"
    do
        if [ ${relationNums[$relation]} -eq 0 ]
        then
            continue
        fi
        flag=1
        if [ $relation -eq 1 ]; then
            echo -n "家庭组人数"
        elif [ $relation -eq 2 ]; then
            echo -n "工作组人数"
        elif [ $relation -eq 3 ]; then
            echo -n "同学组人数"
        else
            echo -n "亲友组人数"
        fi

        echo ": ${relationNums[$relation]}"
    done
    if [ $flag -eq 0 ]; then
        echo "每个关系的人数都是0"
    fi
}

function modify_address_detail() {
    echo "请输入要修改的通讯录编号: "
    read id

    for i in "${!address_book[@]}"
    do
        IFS='|' read -ra ADDR <<< "${address_book[$i]}"
        if [ "${ADDR[0]}" == "$id" ]; then
            echo "当前通讯录信息: ${address_book[$i]}"
            echo "请选择要修改的字段: "
            echo "1. 姓名;"
            echo "2. 关系;"
            echo "3. 手机号码;"
            echo "4. 备注;"
            read field
            case $field in
                1) echo "请输入新的姓名: "; read name; address_book[$i]="${ADDR[0]}|$name|${ADDR[2]}|${ADDR[3]}|${ADDR[4]}";;
                2) echo "请输入新的关系: "; 
                   read relation;
                   rel="${ADDR[2]}";
                   relationNums["$rel"]=$(( ${relationNums["$rel"]} - 1 ));
                   relationNums["$relation"]=$(( ${relationNums["$relation"]} + 1 ));
                   address_book[$i]="${ADDR[0]}|${ADDR[1]}|$relation|${ADDR[3]}|${ADDR[4]}";;
                3) echo "请输入新的手机号码: "; read phone; address_book[$i]="${ADDR[0]}|${ADDR[1]}|${ADDR[2]}|$phone|${ADDR[4]}";;
                4) echo "请输入新的备注: "; read remark; address_book[$i]="${ADDR[0]}|${ADDR[1]}|${ADDR[2]}|${ADDR[3]}|$remark";;
                *) echo "无效的选择，请重新输入";;
            esac
            break
        fi
    done
    
    printf "%s\n" "${address_book[@]}" > "tongxun.txt"
}


function add_address_detail() {
    local id
    local name
    local relation
    local phone
    local remark
    echo "联系人编号是: "
    line_count=$(wc -l < tongxun.txt)
    id=$(( line_count + 1 ))
    echo $id
    read -p "请输入姓名: " name
    while [ -z "$name" ]; do
        read -p "输入不能为空，请重新输入一个字符串: " name
    done
    # echo $id $name
    check_contact_name $name
    if [ $? -eq 0 ]; then 
        add_address_detail
        return 0;
    fi
    # echo $id $name
    read -p "请输入关系: " relation
    while [ -z "$relation" ]; do
        read -p "输入不能为空，请重新输入一个字符串: " relation
    done

    read -p "请输入手机号码: " phone
    while [ -z "$phone" ]; do
        read -p "输入不能为空，请重新输入一个字符串: " phone
    done

    read -p "请输入备注: " remark
    while [ -z "$remark" ]; do
        read -p "输入不能为空，请重新输入一个字符串: " remark
    done

    address_book+=("$id|$name|$relation|$phone|$remark")
    echo "$id|$name|$relation|$phone|$remark" >> tongxun.txt
    refresh_address_book_relationNums $relation
    return 1;
}

function check_contact_name(){
    local check_name
    local id1
    local name1
    local rel1
    local phone1
    local remark1
    check_name=$1
    flag=0
    while IFS='|' read -r id1 name1 rel1 phone1 remark1; do
        if [ "$name1" == "$check_name" ]; then
            flag=1
            break
        fi
    done < tongxun.txt
    if [ "$flag" -eq 1 ]; then
        flag=0
        echo "已存在名为 $name1 的联系人，请重新输入"
        return 0
    else
        return 1
    fi
}

function query_address_detail() {
    echo "要根据编号 姓名 电话来查找联系人?[1/2/3]"
    read field
    if [ -z "${field}" ]; then
        echo "查找出错!"
        return 0;
    elif [ "${field}" -eq 1 ]; then
        echo "请输入联系人编号:"
        read id
        search_contact id $id
        return 1;
    elif [ "${field}" -eq 2 ]; then
        echo "请输入联系人姓名:"
        read name
        search_contact name $name
        return 1;
    elif [ "${field}" -eq 3 ]; then
        echo "请输入联系人电话:"
        read phone
        search_contact phone $phone
        return 1;
    else
        echo "查找出错!"
        return 0;
    fi
}

function search_contact() {
    # 获取要查询的字段和关键字
    field="$1"
    keyword="$2"
    declare -a search_result_array
    # echo "field=$field, keyword=$keyword"
    for i in "${address_book[@]}"
    do
        # -r选项表示禁用反斜杠转义，-a选项表示将读取到的数据存储到数组中
        IFS='|' read -ra ADDR <<< "$i"
        # * ? [] \ 通配符
        if [[ "$field" == "id" && "${ADDR[0]}" == "$keyword" ]] || \
        [[ "$field" == "name" && "${ADDR[1]}" == *"$keyword"* ]] || \
        [[ "$field" == "phone" && "${ADDR[3]}" == *"$keyword"* ]]; then
            search_result_array+=("通讯录编号: ${ADDR[0]}, 姓名: ${ADDR[1]}, 关系: ${ADDR[2]}, 手机号码: ${ADDR[3]}, 备注: ${ADDR[4]}")
        fi
    done

    if [ ${#search_result_array[@]} -eq 0 ]; then
        echo "没有找到匹配的联系人."
        return 0;
    else
        printf "%s\n" "${search_result_array[@]}"
        echo "查找成功!!!^_^"
        echo ""
    fi
   
    
}


function delete_address_detail(){
    read -p "你要根据编号还是姓名来删除联系人?[1/2]" confirm
    if [ "$confirm" == "1" ]
    then
        echo "请输入联系人编号: "
        read id
        delete_address id $id
        return 1
    elif [ "$confirm" == "2" ]
    then
        echo "请输入联系人姓名: "
        read name
        delete_address name $name
        return 1
    else
        echo "删除出错!"
        return 0
    fi
}

function renumber_address_book(){
    # 重新编号通讯录
    local id
    local name
    declare -a new_address_book
    new_id=1
    for contact in "${address_book[@]}"
    do
        # 将原来的id替换成新的id，并将新的联系人信息添加到新的通讯录数组中
        new_contact=$(echo $contact | sed "s/^[0-9]*/$new_id/")
        new_address_book+=("$new_contact")
        
        # 更新新的id值
        new_id=$((new_id + 1))
    done
    
    address_book=("${new_address_book[@]}")
    # echo "${address_book[@]}"
}

function delete_address(){
    field=$1
    keyword=$2
    # echo "field:$field"
    # echo "keyword:$2"
    if [[ "$field" == "id" ]]; then
        temp_id=$2
        for (( i=0; i<${#address_book[@]}; i++ ))
        do
            contact=${address_book[$i]}
            contact_id=$(echo $contact | cut -d '|' -f 1)
            contact_relation=$(echo $contact | cut -d '|' -f 3)
            if [ "$contact_id" == "$temp_id" ]; then
                echo "即将删除以下联系人信息："
                echo "$contact"
                read -p "确认删除该联系人吗？[y/n]" confirm
                if [ "$confirm" == "y" ] || [ "$confirm" == "Y" ]
                then
                    unset address_book[$i]
                    renumber_address_book
                    printf "%s\n" "${address_book[@]}" > "tongxun.txt"
                    refresh_address_book_relationNums $contact_relation
                    echo "已经成功删除(id=$contact_id)的联系人信息！"
                    return 0
                else
                    echo "取消删除操作！"
                    return 1
                fi
            fi
        done
        echo "未找到id为$temp_id的联系人信息！"
        return 1
    elif [[ "$field" == "name" ]]; then
        temp_name=$2
        for (( i=0; i<${#address_book[@]}; i++ ))
        do
            contact=${address_book[$i]}
            contact_name=$(echo $contact | cut -d '|' -f 2)
            contact_relation=$(echo $contact | cut -d '|' -f 3)
            if [ "$contact_name" == "$temp_name" ]; then
                echo "即将删除以下联系人信息："
                echo "$contact"
                read -p "确认删除该联系人吗？[y/n]" confirm
                if [ "$confirm" == "y" ] || [ "$confirm" == "Y" ]
                then
                    unset address_book[$i]
                    renumber_address_book
                    printf "%s\n" "${address_book[@]}" > "tongxun.txt"
                    refresh_address_book_relationNums $contact_relation
                    echo "已经成功删除(name=$contact_name)的联系人信息！"
                    return 1
                else
                    echo "取消删除操作！"
                    return 0
                fi
            fi
        done
        echo "未找到name为$temp_name的联系人信息！"
        return 1
    else
        echo "无效的参数：$1 $2"
        return 0
    fi
    # while getopts ":i:n:" opt; do
    #     case $opt in
    #         i)
    #             # 根据id删除联系人
    #             temp_id=$OPTARG
    #             for (( i=0; i<${#address_book[@]}; i++ ))
    #             do
    #                 contact=${address_book[$i]}
    #                 contact_id=$(echo $contact | cut -d '|' -f 1)
    #                 contact_relation=$(echo $contact | cut -d '|' -f 3)
    #                 if [ "$contact_id" == "$id" ]
    #                 then
    #                     # 输出联系人信息并确认是否删除
    #                     echo "即将删除以下联系人信息："
    #                     echo "$contact"
    #                     read -p "确认删除该联系人吗？[y/n]" confirm
    #                     if [ "$confirm" == "y" ] || [ "$confirm" == "Y" ]
    #                     then
    #                         unset address_book[$i]
    #                         renumber_address_book
    #                         printf "%s\n" "${address_book[@]}" > "tongxun.txt"
    #                         refresh_address_book_relationNums $contact_relation
    #                         echo "已经成功删除(id=$id)的联系人信息！"
    #                         return 0
    #                     else
    #                         echo "取消删除操作！"
    #                         return 1
    #                     fi
    #                 fi
    #             done
    #             echo "未找到id为$id的联系人信息！"
    #             return 1
    #             ;;
    #         n)
    #             # 根据name删除联系人
    #             temp_name=$OPTARG
    #             echo $temp_name
    #             for (( i=0; i<${#address_book[@]}; i++ ))
    #             do
    #                 contact=${address_book[$i]}
    #                 contact_name=$(echo $contact | cut -d '|' -f 2)
    #                 contact_relation=$(echo $contact | cut -d '|' -f 3)
    #                 echo $contact_name
    #                 if [ "$contact_name" == "$name" ]
    #                 then
    #                     # 输出联系人信息并确认是否删除
    #                     echo "即将删除以下联系人信息："
    #                     echo "$contact"
    #                     read -p "确认删除该联系人吗？[y/n]" confirm
    #                     if [ "$confirm" == "y" ] || [ "$confirm" == "Y" ]
    #                     then
    #                         unset address_book[$i]
    #                         renumber_address_book
    #                         printf "%s\n" "${address_book[@]}" > "tongxun.txt"
    #                         refresh_address_book_relationNums $contact_relation
    #                         echo "已经成功删除(name=$name)的联系人信息！"
    #                         return 0
    #                     else
    #                         echo "取消删除操作！"
    #                         return 1
    #                     fi
    #                 fi
    #             done
    #             echo "未找到名字为$name的联系人信息！"
    #             return 1
    #             ;;
    #         \?)
    #             # 处理无效参数
    #             echo "无效的参数：-$OPTARG"
    #             return 1
    #             ;;
    #     esac
    # done
}

function import_file(){
    read -p "请输入文件路径: " file_path

    # 判断文件是否存在
    if [ ! -f "$file_path" ]; then
        echo "文件未找到."
        exit 1
    fi

    # 逐行读取文件内容，并将每行内容转换为数组格式
    while read line; do
        IFS='|' read -r -a fields <<< "$line"
        id=${fields[0]}
        name=${fields[1]}
        relation=${fields[2]}
        phone=${fields[3]}
        remark=${fields[4]}
        
        # 检查id是否为null
        if [ -n "$id" ]; then
            address_book+=("$id|$name|$relation|$phone|$remark")
        fi
    done <"$file_path"

    renumber_address_book
    save_address_book
    echo "导入成功!!!^_^"

}

# # 打印删除后的通讯录信息
# echo "删除后的通讯录："
# echo "${address_book[@]}"


# 加载已有的通讯录
load_address_book

while true
do
    echo "欢迎使用通讯录系统，请输入对应数字选择功能: "
    echo "1. 添加通讯录关系信息"
    echo "2. 查询所有通讯录关系信息"
    echo "3. 添加通讯录详情信息"
    echo "4. 查询通讯录详情信息"
    echo "5. 删除联系人详情信息"
    echo "6. 修改联系人详情信息"
    echo "7. 导入联系人"
    echo "8. 备份通讯录,并且通过OpenSSL加密"
    echo "9. 解密备份文件"
    echo "10. 退出程序"

    read choice
    case $choice in
        1) add_address_book;;
        2) query_address_book;;
        3) add_address_detail;;
        4) query_address_detail;;
        5) delete_address_detail;;
        6) modify_address_detail;;
        7) import_file;;
        8) backup_address_book;;
        9) backup_address_book_decrypted;;
        10) save_address_book; echo "成功退出!!!^_^"; exit 0;;
        *) echo "无效的选择，请重新输入";;
    esac
done
```

# 1、设计与实现

## 1.1 功能实现

​	①添加通讯录关系信息；

​	②查询所有通讯录关系信息；

​	③添加通讯录详情信息；

​	④查询通讯录详情信息；

​	⑤删除联系人详情信息；

​	⑥修改联系人详情信息；

​	⑦导入联系人；

⑧备份通讯录,并且通过OpenSSL加密；

⑨解密备份文件；

⑩退出程序；

## 1.2 部分功能代码和解释

### 1.2.1 捕捉型号

```shell
trap save_address_book INT;
```

trap 是 shell 内置命令，用于捕获并处理信号；

save_address_book 是一个保存通讯录函数，表示在接收到指定信号时要执行保存通讯录的的操作；

INT 是指要捕获的信号类型，这里是中断信号（即按下 Ctrl+C 键）。

 

该命令的作用是当用户在运行脚本时按下 Ctrl+C 键时，会触发中断信号，此时 trap 命令会捕获该信号，并执行 save_address_book 函数中定义的保存地址薄的操作，以便在脚本中进行异常处理。

 

在实际使用中，也可以根据需要设置不同的信号类型和对应的操作函数，以便在发生特定事件时进行相应的处理。

 

### 1.2.2 删除联系人详情信息

```shell
function delete_address_detail(){
    read -p "你要根据编号还是姓名来删除联系人?[1/2]" confirm
    if [ "$confirm" == "1" ]
    then
        echo "请输入联系人编号: "
        read id
        delete_address id $id
        return 1
    elif [ "$confirm" == "2" ]
    then
        echo "请输入联系人姓名: "
        read name
        delete_address name $name
        return 1
    else
        echo "删除出错!"
        return 0
    fi
}
function delete_address(){
    field=$1
    keyword=$2
    # echo "field:$field"
    # echo "keyword:$2"
    if [[ "$field" == "id" ]]; then
        temp_id=$2
        for (( i=0; i<${#address_book[@]}; i++ ))
        do
            contact=${address_book[$i]}
            contact_id=$(echo $contact | cut -d '|' -f 1)
            contact_relation=$(echo $contact | cut -d '|' -f 3)
            if [ "$contact_id" == "$temp_id" ]; then
                echo "即将删除以下联系人信息："
                echo "$contact"
                read -p "确认删除该联系人吗？[y/n]" confirm
                if [ "$confirm" == "y" ] || [ "$confirm" == "Y" ]
                then
                    unset address_book[$i]
                    renumber_address_book
                    printf "%s\n" "${address_book[@]}" > "tongxun.txt"
                    refresh_address_book_relationNums $contact_relation
                    echo "已经成功删除(id=$contact_id)的联系人信息！"
                    return 0
                else
                    echo "取消删除操作！"
                    return 1
                fi
            fi
        done
        echo "未找到id为$temp_id的联系人信息！"
        return 1
    elif [[ "$field" == "name" ]]; then
        temp_name=$2
        for (( i=0; i<${#address_book[@]}; i++ ))
        do
            contact=${address_book[$i]}
            contact_name=$(echo $contact | cut -d '|' -f 2)
            contact_relation=$(echo $contact | cut -d '|' -f 3)
            if [ "$contact_name" == "$temp_name" ]; then
                echo "即将删除以下联系人信息："
                echo "$contact"
                read -p "确认删除该联系人吗？[y/n]" confirm
                if [ "$confirm" == "y" ] || [ "$confirm" == "Y" ]
                then
                    unset address_book[$i]
                    renumber_address_book
                    printf "%s\n" "${address_book[@]}" > "tongxun.txt"
                    refresh_address_book_relationNums $contact_relation
                    echo "已经成功删除(name=$contact_name)的联系人信息！"
                    return 1
                else
                    echo "取消删除操作！"
                    return 0
                fi
            fi
        done
        echo "未找到name为$temp_name的联系人信息！"
        return 1
    else
        echo "无效的参数：$1 $2"
        return 0
    fi
}
```

通过这两个函数组合起来删除联系人，删除通讯录联系人的函数是 delete_address_detail()，组合它的另一个函数 delete_address()。

 

在 delete_address_detail() 函数中，首先读取用户输入的删除方式（编号或姓名），然后根据不同的选择调用 delete_address() 函数进行删除操作。如果用户输入的选项无效，则输出错误信息并返回。

 

在 delete_address() 函数中，使用了 getopts 命令解析命令行参数。

如果用户选择根据编号删除联系人，则遍历地址薄数组 address_book[] 中的每一个元素，找到与指定编号相匹配的联系人，并输出该联系人信息，让用户确认是否删除。如果用户确认删除，则从数组中删除该联系人信息，并重新对地址薄进行编号。最后，将更新后的地址薄写回到文件中，刷新联系人编号，输出删除成功的信息。

如果用户选择根据姓名删除联系人，则采用类似的方法查找和删除联系人信息。需要注意的是，此时比较的是联系人姓名而不是编号。

 

### 1.2.3 导入通讯录联系人

```shell
function import_file(){
    read -p "请输入文件路径: " file_path

    # 判断文件是否存在
    if [ ! -f "$file_path" ]; then
        echo "文件未找到."
        exit 1
    fi

    # 逐行读取文件内容，并将每行内容转换为数组格式
    while read line; do
        IFS='|' read -r -a fields <<< "$line"
        id=${fields[0]}
        name=${fields[1]}
        relation=${fields[2]}
        phone=${fields[3]}
        remark=${fields[4]}
        
        # 将联系人添加到通讯录中
        if [ -n "$id" ]; then
            address_book+=("$id|$name|$relation|$phone|$remark")
        fi
    done <"$file_path"

    renumber_address_book
    save_address_book
    echo "导入成功!!!^_^"
}
```

使用固定的格式来导入通讯录联系人的函数 import_file()

 

函数首先通过 read 命令从用户输入中获取要导入的文件路径。然后，它检查该文件是否存在，如果不存在则输出错误信息并退出函数。

 

接着，函数使用 while read 命令逐行读取文件内容，并将每行内容按照竖线分隔符 | 转换为数组格式。然后，函数从数组中提取联系人的各个字段（编号、姓名、关系、电话、备注），并将其添加到通讯录数组 address_book[] 中。需要注意的是，如果该联系人的编号为空，则不会将其添加到地址薄中。

 

最后，函数对地址薄进行重新编号，保存更新后的地址薄数据到文件中，并输出导入成功的信息。

 

### 1.2.4 备份联系人以及加密备份文件

```shell
# 备份通讯录
function backup_address_book() {
    # 备份通讯录数据到文件
    touch address_book_backup.txt
    printf "%s\n" "${address_book[@]}" > "address_book_backup.txt"

    # 加密备份文件
    openssl aes-256-cbc -salt -in address_book_backup.txt -out address_book_encrypted.bin

    # 删除明文备份文件
    rm address_book_backup.txt

    # # 解密备份文件
    # read -p "请输入解密密码：" password
    # openssl aes-256-cbc -d -salt -in address_book_encrypted.bin -out address_book_decrypted.txt -pass pass:$password

    echo "备份已完成。"
}

# 解密备份文件
function backup_address_book_decrypted(){
    read -p "请输入解密密码：" password
    openssl aes-256-cbc -d -salt -in address_book_encrypted.bin -out address_book_decrypted.txt -pass pass:$password

    rm address_book_encrypted.bin
    echo "备份已解密。"
}
```

在backup_address_book() 函数中，首先使用 touch 命令创建一个名为 address_book_backup.txt 的空文件，然后使用 printf 命令将通讯录数组 address_book[] 中的所有元素写入该文件。接着，函数使用 OpenSSL 命令行工具对备份文件进行 AES-256 加密，并将加密后的文件保存为 address_book_encrypted.bin。最后，函数删除明文备份文件并输出备份完成的信息。

 

在 backup_address_book_decrypted() 函数中，用户需要输入解密密码，然后使用 OpenSSL 命令行工具对加密备份文件 address_book_encrypted.bin 进行 AES-256 解密，并将解密后的数据保存到文件 address_book_decrypted.txt 中。最后，函数删除加密备份文件并输出备份已解密的信息。

 

其中

```shell
openssl aes-256-cbc -salt -in address_book_backup.txt -out address_book_encrypted.bin
```

这条命令的含义是：

openssl 是加密工具，它是一个开源的安全套接字层协议库，提供了加密、解密、数字签名等各种安全相关的功能；

aes-256-cbc 是加密算法，表示使用 256 位密钥进行 AES 加密，并采用 CBC 模式；

-d 表示解密操作；

-salt 表示在加密和解密过程中使用随机生成的盐值；

-in address_book_encrypted.bin 表示要解密的输入文件为 address_book_encrypted.bin，即加密后的地址薄文件；

-out address_book_decrypted.txt 表示解密后输出的文件名为 address_book_decrypted.txt，即解密后得到的地址薄明文；

-pass pass:$password 表示使用密码 $password 来解密文件。注意，在这里 $password 应该替换成你实际使用的密码。

 

```shell
openssl aes-256-cbc -d -salt -in address_book_encrypted.bin -out address_book_decrypted.txt -pass pass:$password
```

aes-256-cbc 是加密算法，表示使用 256 位密钥进行 AES 加密，并采用 CBC 模式；

-d 表示解密操作；

-salt 表示在加密和解密过程中使用随机生成的盐值；

-in address_book_encrypted.bin 表示要解密的输入文件为 address_book_encrypted.bin，即加密后的地址薄文件；

-out address_book_decrypted.txt 表示解密后输出的文件名为 address_book_decrypted.txt，即解密后得到的地址薄明文；

-pass pass:$password 表示使用密码 $password 来解密文件。注意，在这里 $password 应该替换成你实际使用的密码。

 

 
