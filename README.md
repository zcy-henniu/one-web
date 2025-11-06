#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
#include <<iomanip>
#include <limits>

// 定义药品食材相互作用结构体
struct Interaction {
    std::string drugName;        // 药品通用名
    std::string foodName;        // 食材名称
    std::string interactionType; // 相互作用类型
    std::string riskLevel;       // 风险等级（high/medium/low）
    std::string mechanism;       // 作用机制简述
    std::string suggestion;      // 推荐建议
    std::string dataSource;      // 数据来源
    std::string drugCategory;    // 药品分类
    std::string foodCategory;    // 食材分类
};

// 初始化相互作用数据（包含新增的6组数据）
std::vector<Interaction> initInteractionData() {
    return {
        // 原有数据
        {"辛伐他汀", "西柚（葡萄柚）", "代谢抑制", "high", "西柚抑制CYP3A4酶，增加药物血药浓度", "服药期间严禁食用西柚及制品", "湘雅医院临床研究", "statins", "fruits"},
        {"华法林", "菠菜", "拮抗作用", "medium", "菠菜含大量维生素K，影响抗凝效果", "每日摄入量需固定，避免骤增骤减", "湘雅医院临床研究", "anticoagulants", "vegetables"},
        {"硝苯地平", "芹菜", "协同辅助", "low", "芹菜含芹菜素，辅助舒张血管", "可适量搭配食用，增强降压辅助效果", "湘雅医院临床研究", "antihypertensives", "vegetables"},
        {"二甲双胍", "苦瓜", "协同辅助", "low", "苦瓜含苦瓜皂苷，辅助改善胰岛素敏感性", "可作为日常蔬菜搭配食用", "湘雅医院临床研究", "antidiabetics", "vegetables"},
        {"阿司匹林", "酒精", "毒性增强", "high", "酒精刺激胃黏膜，加重药物致胃出血风险", "服药期间严格禁酒", "湘雅医院临床研究", "analgesics", "alcohol"},
        // 新增数据
        {"氟西汀", "酪胺类食物（如奶酪）", "代谢抑制", "high", "抑制MAO酶，酪胺代谢受阻，导致血压骤升", "服药期间避免食用含大量酪胺的食物", "湘雅医院临床研究", "antidepressants", "grains"},
        {"四环素", "牛奶", "络合作用", "medium", "牛奶中的钙与药物形成络合物，降低吸收率", "服药前后2小时避免食用乳制品", "湘雅医院临床研究", "antibiotics", "dairy"},
        {"卡托普利", "高盐食物", "拮抗作用", "medium", "高盐摄入增加血容量，降低降压效果", "低盐饮食，每日盐摄入量控制在6g以内", "湘雅医院临床研究", "antihypertensives", "grains"},
        {"阿卡波糖", "膳食纤维丰富食物", "协同辅助", "low", "膳食纤维延缓碳水化合物吸收，辅助控制血糖", "可适量搭配食用，有助于血糖平稳", "湘雅医院临床研究", "antidiabetics", "grains"},
        {"华法林", "酒精", "酶诱导/抑制", "high", "酒精影响肝酶活性，增强或减弱抗凝效果", "服药期间严格禁酒", "湘雅医院临床研究", "anticoagulants", "alcohol"},
        {"头孢类", "酒精", "双硫仑样反应", "medium", "抑制乙醛脱氢酶，导致乙醛蓄积，引发不适", "服药期间及停药后3天内避免饮酒", "湘雅医院临床研究", "antibiotics", "alcohol"}
    };
}

// 显示药品分类菜单
int showDrugCategoryMenu() {
    std::cout << "\n=== 药品分类筛选 ===" << std::endl;
    std::cout << "1. 全部" << std::endl;
    std::cout << "2. 他汀类（statins）" << std::endl;
    std::cout << "3. 抗凝药（anticoagulants）" << std::endl;
    std::cout << "4. 降压药（antihypertensives）" << std::endl;
    std::cout << "5. 降糖药（antidiabetics）" << std::endl;
    std::cout << "6. 镇痛药（analgesics）" << std::endl;
    std::cout << "7. 抗抑郁药（antidepressants）" << std::endl;
    std::cout << "8. 抗生素（antibiotics）" << std::endl;
    std::cout << "请选择药品分类（输入序号）：";
    int choice;
    std::cin >> choice;
    // 输入验证
    while (choice < 1 || choice > 8) {
        std::cout << "输入无效，请重新选择（1-8）：";
        std::cin.clear();
        std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
        std::cin >> choice;
    }
    return choice;
}

// 显示食材分类菜单
int showFoodCategoryMenu() {
    std::cout << "\n=== 食材分类筛选 ===" << std::endl;
    std::cout << "1. 全部" << std::endl;
    std::cout << "2. 水果（fruits）" << std::endl;
    std::cout << "3. 蔬菜（vegetables）" << std::endl;
    std::cout << "4. 酒精（alcohol）" << std::endl;
    std::cout << "5. 乳制品（dairy）" << std::endl;
    std::cout << "6. 谷物（grains）" << std::endl;
    std::cout << "请选择食材分类（输入序号）：";
    int choice;
    std::cin >> choice;
    // 输入验证
    while (choice < 1 || choice > 6) {
        std::cout << "输入无效，请重新选择（1-6）：";
        std::cin.clear();
        std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
        std::cin >> choice;
    }
    return choice;
}

// 显示风险等级菜单
int showRiskLevelMenu() {
    std::cout << "\n=== 风险等级筛选 ===" << std::endl;
    std::cout << "1. 全部" << std::endl;
    std::cout << "2. 红色高风险（high）" << std::endl;
    std::cout << "3. 黄色中风险（medium）" << std::endl;
    std::cout << "4. 绿色低风险（low）" << std::endl;
    std::cout << "请选择风险等级（输入序号）：";
    int choice;
    std::cin >> choice;
    // 输入验证
    while (choice < 1 || choice > 4) {
        std::cout << "输入无效，请重新选择（1-4）：";
        std::cin.clear();
        std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
        std::cin >> choice;
    }
    return choice;
}

// 根据用户选择获取对应分类字符串
std::string getCategoryStr(int choice, const std::vector<std::string>& categories) {
    if (choice >= 1 && choice <= categories.size()) {
        return categories[choice - 1];
    }
    return "all";
}

// 执行筛选并显示结果
void applyFiltersAndShowResults(const std::vector<Interaction>& data) {
    // 获取用户筛选条件
    int drugChoice = showDrugCategoryMenu();
    std::vector<std::string> drugCategories = {"all", "statins", "anticoagulants", "antihypertensives", "antidiabetics", "analgesics", "antidepressants", "antibiotics"};
    std::string selectedDrug = getCategoryStr(drugChoice, drugCategories);

    int foodChoice = showFoodCategoryMenu();
    std::vector<std::string> foodCategories = {"all", "fruits", "vegetables", "alcohol", "dairy", "grains"};
    std::string selectedFood = getCategoryStr(foodChoice, foodCategories);

    int riskChoice = showRiskLevelMenu();
    std::vector<std::string> riskLevels = {"all", "high", "medium", "low"};
    std::string selectedRisk = getCategoryStr(riskChoice, riskLevels);

    // 关键词搜索
    std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n'); // 忽略之前的换行
    std::string searchTerm;
    std::cout << "\n请输入药品或食材关键词（可留空）：";
    std::getline(std::cin, searchTerm);
    // 转换为小写，便于不区分大小写搜索
    std::transform(searchTerm.begin(), searchTerm.end(), searchTerm.begin(), ::tolower);

    // 筛选数据
    std::vector<Interaction> filteredData;
    for (const auto& item : data) {
        bool match = true;

        // 药品分类筛选
        if (selectedDrug != "all" && item.drugCategory != selectedDrug) {
            match = false;
        }

        // 食材分类筛选
        if (selectedFood != "all" && item.foodCategory != selectedFood) {
            match = false;
        }

        // 风险等级筛选
        if (selectedRisk != "all" && item.riskLevel != selectedRisk) {
            match = false;
        }

        // 关键词搜索筛选（不区分大小写）
        if (!searchTerm.empty()) {
            std::string drugLower = item.drugName;
            std::string foodLower = item.foodName;
            std::transform(drugLower.begin(), drugLower.end(), drugLower.begin(), ::tolower);
            std::transform(foodLower.begin(), foodLower.end(), foodLower.begin(), ::tolower);
            if (drugLower.find(searchTerm) == std::string::npos && foodLower.find(searchTerm) == std::string::npos) {
                match = false;
            }
        }

        if (match) {
            filteredData.push_back(item);
        }
    }

    // 显示筛选结果
    std::cout << "\n=== 筛选结果 ===" << std::endl;
    if (filteredData.empty()) {
        std::cout << "未找到匹配的药品食材相互作用数据！" << std::endl;
        return;
    }

    // 设置表格列宽，优化显示
    const int colWidth1 = 15;
    const int colWidth2 = 20;
    const int colWidth3 = 15;
    const int colWidth4 = 15;
    const int colWidth5 = 30;
    const int colWidth6 = 30;
    const int colWidth7 = 20;

    // 表头
    std::cout << std::left
              << std::setw(colWidth1) << "药品通用名"
              << std::setw(colWidth2) << "食材名称"
              << std::setw(colWidth3) << "相互作用类型"
              << std::setw(colWidth4) << "风险等级"
              << std::setw(colWidth5) << "作用机制简述"
              << std::setw(colWidth6) << "推荐建议"
              << std::setw(colWidth7) << "数据来源" << std::endl;

    // 分隔线
    std::cout << std::string(colWidth1 + colWidth2 + colWidth3 + colWidth4 + colWidth5 + colWidth6 + colWidth7, '-') << std::endl;

    // 数据行（根据风险等级标注颜色提示）
    for (const auto& item : filteredData) {
        std::string riskDesc;
        if (item.riskLevel == "high") {
            riskDesc = "红色高风险";
        } else if (item.riskLevel == "medium") {
            riskDesc = "黄色中风险";
        } else {
            riskDesc = "绿色低风险";
        }

        std::cout << std::left
                  << std::setw(colWidth1) << item.drugName
                  << std::setw(colWidth2) << item.foodName
                  << std::setw(colWidth3) << item.interactionType
                  << std::setw(colWidth4) << riskDesc
                  << std::setw(colWidth5) << item.mechanism
                  << std::setw(colWidth6) << item.suggestion
                  << std::setw(colWidth7) << item.dataSource << std::endl;
    }
}

// 主菜单
int showMainMenu() {
    std::cout << "\n=== 药品食材相互作用查询系统 ===" << std::endl;
    std::cout << "1. 开始查询" << std::endl;
    std::cout << "2. 查看所有数据" << std::endl;
    std::cout << "3. 退出系统" << std::endl;
    std::cout << "请选择操作（输入序号）：";
    int choice;
    std::cin >> choice;
    // 输入验证
    while (choice < 1 || choice > 3) {
        std::cout << "输入无效，请重新选择（1-3）：";
        std::cin.clear();
        std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
        std::cin >> choice;
    }
    return choice;
}

// 查看所有数据
void showAllData(const std::vector<Interaction>& data) {
    std::cout << "\n=== 所有药品食材相互作用数据 ===" << std::endl;
    if (data.empty()) {
        std::cout << "暂无数据！" << std::endl;
        return;
    }

    // 表格格式（同筛选结果）
    const int colWidth1 = 15;
    const int colWidth2 = 20;
    const int colWidth3 = 15;
    const int colWidth4 = 15;
    const int colWidth5 = 30;
    const int colWidth6 = 30;
    const int colWidth7 = 20;

    std::cout << std::left
              << std::setw(colWidth1) << "药品通用名"
              << std::setw(colWidth2) << "食材名称"
              << std::setw(colWidth3) << "相互作用类型"
              << std::setw(colWidth4) << "风险等级"
              << std::setw(colWidth5) << "作用机制简述"
              << std::setw(colWidth6) << "推荐建议"
              << std::setw(colWidth7) << "数据来源" << std::endl;

    std::cout << std::string(colWidth1 + colWidth2 + colWidth3 + colWidth4 + colWidth5 + colWidth6 + colWidth7, '-') << std::endl;

    for (const auto& item : data) {
        std::string riskDesc = (item.riskLevel == "high") ? "红色高风险" :
                              (item.riskLevel == "medium") ? "黄色中风险" : "绿色低风险";

        std::cout << std::left
                  << std::setw(colWidth1) << item.drugName
                  << std::setw(colWidth2) << item.foodName
                  << std::setw(colWidth3) << item.interactionType
                  << std::setw(colWidth4) << riskDesc
                  << std::setw(colWidth5) << item.mechanism
                  << std::setw(colWidth6) << item.suggestion
                  << std::setw(colWidth7) << item.dataSource << std::endl;
    }
}

int main() {
    // 初始化数据
    std::vector<Interaction> interactionData = initInteractionData();

    while (true) {
        int mainChoice = showMainMenu();
        switch (mainChoice) {
            case 1:
                // 开始查询（执行筛选）
                applyFiltersAndShowResults(interactionData);
                break;
            case 2:
                // 查看所有数据
                showAllData(interactionData);
                break;
            case 3:
                // 退出系统
                std::cout << "\n感谢使用药品食材相互作用查询系统，再见！" << std::endl;
                return 0;
            default:
                std::cout << "输入无效，请重新选择！" << std::endl;
                break;
        }

        // 暂停，等待用户查看结果
        std::cout << "\n按Enter键返回主菜单...";
        std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
        std::cin.get();
    }
}
