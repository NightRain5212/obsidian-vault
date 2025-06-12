

# 华东师范大学计算机科学技术系项目报告

-   **课程名称**：数据结构
-   **指导教师**：肖春芸
-   **姓名**：吴俊达
-   **学号**：10245102514
-   **年级**：24级
-   **日期**：2025/6/3

---

## 一、 项目名称

**校园导航系统**

## 二、 项目仓库

-   **仓库地址**：[NightRain5212/CampusNavigation](https://github.com/NightRain5212/CampusNavigation)
-   **所用语言**：**C++** (94.8%), Makefile(3.7%), **CMake** (1.3%), **C** (0.2%)

## 三、项目结构

本项目主要使用 QT 框架进行开发，核心代码结构清晰，易于维护和扩展。

-   **`.ui`**：通过 Qt Designer 设计的 UI 界面文件，定义了窗口和控件的布局。
-   **`.h`**：C++ 头文件，用于声明类、接口和数据结构。
    -   `graph.h`：项目的核心，定义了图的数据结构（邻接表、地点节点、边）和所有相关算法的接口。
    -   `mainwindow.h`：主窗口类的头文件，负责主界面的逻辑交互。
    -   `*dialog.h`：各类功能对话框的头文件，用于接收用户输入（如添加地点、查询路径等）。
-   **`.cpp`**：C++ 源文件，包含各个头文件中声明的函数和类的具体实现。
-   **`main.cpp`**: 程序的入口点，负责创建和显示主窗口。

## 四、 主要功能

本项目实现了一个功能完备的校园导航系统，主要功能如下:

1.  **节点和边的基本操作**：
    -   **插入节点**：支持添加新的地点信息（名称、类型、建议游玩时间）。
    -   **删除节点**：支持安全地删除指定的地点及其所有关联道路。
    -   **查找节点**：支持按地点名称、类型等关键字进行查找。
    -   **更新节点**：支持更新地点的参观时间等信息。
    -   **插入边**：支持在两个地点之间添加新的道路信息。
    -   **删除边**：支持删除指定的道路。
    -   **查找边**：支持查找与特定地点相连的所有道路。
    -   **更新边**：支持更新道路的权重（长度）。
2.  **文件操作** ：
    -   **智能读取**：能够从 CSV 文件读取道路和地点信息，并自动跳过第一行的表头，兼容带标题的数据表。
    -   **数据持久化**：支持将当前图中的所有地点和道路信息，分别保存到两个结构化的 CSV 文件中。
3.  **操作选项**：
    -   **信息展示**：可分类显示所有地点和所有道路的详细信息。
    -   **邻接表打印**：以边集的形式清晰地打印出图的邻接表结构。
    -   **一键重置**：支持一键清空图中所有数据，方便重新加载。
4.  **核心算法实现**：
    -   **连通性判断**：基于并查集（Union-Find）算法，高效判断整个校园地图是否连通。若不连通，则会给出推荐添加的路径使其连通。
    -   **欧拉路径判断与求解**：判断是否存在一笔画走完所有道路的路径，并能实际计算出该路径、总长度以及沿途所有地点的建议游玩总时间。
    -   **最短路径计算 (Dijkstra)**：利用优先队列优化的 Dijkstra 算法，快速计算两点之间的最短路径，并给出路径详情、总长度和推荐游玩时间。
    -   **最小生成树 (Kruskal)**：求解连接所有地点的最小代价路径网络，可用于校园网络布线、管道铺设等场景的模拟。
5.  **用户交互界面**：
    -   基于 QT 实现功能完善的图形用户界面（GUI）。
    -   主界面实时显示已构建图中的地点数和边数。
    -   设有详细的**日志窗口**，所有操作（无论成功或失败）都会生成带时间戳的反馈信息。
    -   所有日志会自动保存到本地 `log.txt` 文件中，方便调试和追踪。

## 五、 其他部分

### 1. 性能分析

本项目的性能主要体现在算法效率和用户体验两个方面。

-   **算法时间复杂度**：
    -   **图的表示**：采用**邻接表**存储图，空间复杂度为 $O(V+E)$（V为顶点数，E为边数），对于地点多、道路相对稀疏的校园地图非常节省空间。
    -   **最短路径**：使用**优先队列优化的 Dijkstra 算法**，时间复杂度为 $O(E \log V)$，能够快速响应用户的路径查询请求。
    -   **最小生成树**：采用 **Kruskal 算法**，通过对边进行快速排序并结合**并查集**进行判断，时间复杂度为 $O(E \log E)$，效率很高。
    -   **欧拉路径**：判断和求解过程主要涉及遍历图的度和连通性判断，时间复杂度为 $O(V+E)$，是线性时间算法。
    -   **地点名称查找**：通过 `std::map` 将地点名称映射到内部ID，使查找操作的时间复杂度稳定在 $O(\log V)$，避免了耗时的线性搜索。

-   **用户体验与响应速度**：
    -   **内存使用**：邻接表的存储方式有效控制了内存占用。在删除节点时，采用**懒惰删除**（将节点标记为无效而非物理删除）策略，避免了因大规模数据迁移导致的性能抖动和ID错乱问题，保证了程序的稳定性。

### 2. 遇到的主要问题

在开发过程中，主要遇到了以下几个挑战，并通过相应的设计和技术方案得以解决：

1.  **问题：节点删除的复杂性与数据一致性**
    -   **描述**：在邻接表结构中，物理删除一个节点（例如，从 `std::vector` 中 `erase`）会导致后续所有节点的索引（ID）发生改变，这将使得所有与这些节点相关的边信息全部失效，引发数据错乱，修复成本极高。
    -   **解决方案**：采用了**懒惰删除（Soft Deletion）**策略。在 `PlaceNode` 结构体中增加一个 `bool isValid` 标志位。当用户删除一个地点时，程序仅将该地点的 `isValid` 设为 `false`，并将其从名称-ID映射表 `placeNameToIdMap` 中移除。同时，遍历全图，移除所有指向该无效节点的边。在所有其他算法（如寻路、显示）中，都会检查节点的 `isValid` 标志，从而忽略这些“已删除”的节点。该方案以微小的逻辑开销，保证了节点ID的永久稳定性和数据结构的一致性。

2.  **问题：算法对非连通图的适应性**
    -   **描述**：最小生成树和欧拉路径等算法通常要求图是连通的。如果用户加载的数据本身包含多个不相连的区域（例如，一个主校区和一个相隔很远的家属区），直接运行这些算法会产生错误或无意义的结果。
    -   **解决方案**：在执行这些核心算法前，强制进行**连通性检测**。我们实现了一个高效的**并查集（Union-Find）数据结构来判断图的连通分量数量。如果图不连通，程序会立刻向用户返回明确的提示信息（如：“非连通图无法生成最小生成树”），而不是继续执行并导致程序崩溃或逻辑错误，大大增强了程序的健壮性。**

## 六、 代码示例

-   **最短路径 (Dijkstra 算法实现)**
    -   该函数是项目核心算法之一，展示了如何结合优先队列、邻接表和自定义数据结构来高效地解决单源最短路径问题。


```cpp
void Graph::findShortestPath(const QString &startName, const QString &endName)
{
    // ... (前置的有效性检查) ...
    if (placeNameToIdMap.count(startName) == 0 ||
        static_cast<size_t>(placeNameToIdMap[startName]) >= places.size() ||
        !places[placeNameToIdMap[startName]].isValid) {
        QString msg = QString("错误：起始地点 '%1' 不存在或无效。\n").arg(startName);
        emit newLog(msg, "ERROR");
        qDebug().noquote() << msg;
        return;
    }
    if (placeNameToIdMap.count(endName) == 0 ||
        static_cast<size_t>(placeNameToIdMap[endName]) >= places.size() ||
        !places[placeNameToIdMap[endName]].isValid) {
        QString msg = QString("错误：目标地点 '%1' 不存在或无效。\n").arg(endName);
        emit newLog(msg, "ERROR");
        qDebug().noquote() << msg;
        return;
    }
    int startId = placeNameToIdMap[startName];
    int endId = placeNameToIdMap[endName];
    int time = 0;
    // ... (处理起点终点相同的情况) ...
    if (startId == endId) {
        QString msg = QString("路径查找：起始地点和目标地点相同 ('%1')。\n路径长度：0。\n路径：%1\n")
                          .arg(startName);
        emit newLog(msg, "INFO");
        qDebug().noquote() << msg;
        return;
    }
    size_t nodeCount = places.size();
    std::vector<long long> dist(nodeCount, LLONG_MAX);
    std::vector<int> pre(nodeCount, -1);

    // 优先队列优化Dijkstra，存储{-距离, 节点ID}，负距离用于实现最小堆
    std::priority_queue<std::pair<long long, int>> pq;

    dist[startId] = 0;
    pq.push({0, startId});

    while (!pq.empty()) {
        long long d = -pq.top().first; // 恢复正距离
        int u = pq.top().second;
        pq.pop();

        // 如果找到了更短的路径，则跳过
        if (d > dist[u]) {
            continue;
        }
        
        // 遍历当前节点u的所有邻居v
        if (static_cast<size_t>(u) < adjacencyList.size()) {
            for (const auto& edge : adjacencyList[u]) {
                int v = edge.toNodeId;
                int w = edge.length;

                // 忽略无效节点
                if (static_cast<size_t>(v) >= nodeCount || !places[v].isValid) {
                    continue;
                }

                // 松弛操作：如果经由u到v的路径更短
                if (dist[u] != LLONG_MAX && dist[u] + w < dist[v]) {
                    dist[v] = dist[u] + w;
                    pre[v] = u; // 记录前驱节点
                    pq.push({-dist[v], v}); // 将更新后的节点加入队列
                }
            }
        }
    }

    // ... (路径回溯与结果输出) ...
    if (dist[endId] == LLONG_MAX) { // 如果目标地点不可达
        QString msg = QString("未能找到从 '%1' 到 '%2' 的路径。\n").arg(startName).arg(endName);
        emit newLog(msg, "WARNING");
        qDebug().noquote() << msg;
    } else {
        std::vector<QString> pathVec;
        int pNode = endId;
        while (pNode != -1) {
            time+=places[pNode].recommendedTime;
            pathVec.push_back(places[pNode].name);
            pNode = pre[pNode];
        }

        // 检查路径重建是否成功
        bool pathReconstructionError = pathVec.empty() || (pathVec.size() == 1 && pathVec.front().startsWith("[错误"));
        if (pathReconstructionError && startId != endId) { // 如果起点终点不同但路径重建失败
            QString msg = QString("错误：无法重建从 '%1' 到 '%2' 的有效路径，尽管计算出距离为 %3。\n")
                              .arg(startName).arg(endName).arg(dist[endId]);
            emit newLog(msg, "ERROR");
            qDebug().noquote() << msg;
            return;
        }


        std::reverse(pathVec.begin(), pathVec.end());

        QString sep = " --> ";
        QString pathText;
        if (!pathVec.empty()) {
            pathText = pathVec[0]; // 先添加第一个元素
            for (size_t i = 1; i < pathVec.size(); ++i) {
                pathText += sep + pathVec[i]; // 从第二个元素开始，前面加上分隔符
            }
        }

        QString msg = QString("从 '%1' 到 '%2' 的最短路径：\n总长度：%3 米\n路径：%4\n推荐游玩时间：%5\n")
                          .arg(startName)
                          .arg(endName)
                          .arg(dist[endId])
                          .arg(pathText)
                          .arg(time);
        emit newLog(msg, "INFO");
        qDebug().noquote() << msg;
    }
}
```



## 七、总结

通过本次数据结构课程项目，我独立设计并实现了一个功能较为全面的校园导航系统。在开发过程中，我不仅加深了对图、邻接表、并查集等核心数据结构的理解，还熟练掌握了 Dijkstra、Kruskal、欧拉路径等经典图算法的具体应用。

更重要的是，我将理论知识与实际工程问题相结合，通过懒惰删除策略保证了数据一致性，并利用 Qt 框架构建了良好的人机交互界面。整个项目从需求分析、结构设计、代码实现到最终的报告撰写，全面锻炼了我的软件工程能力和解决复杂问题的能力，收获颇丰。