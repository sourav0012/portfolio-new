# portfolio-new
UI-UX PORTFOLIO
import React, { useState, useMemo } from "react";

const DataTable = ({ data, columns }) => {
  const [searchText, setSearchText] = useState("");
  const [sortConfig, setSortConfig] = useState({ key: null, direction: "asc" });

  const handleSort = (key) => {
    let direction = "asc";
    if (sortConfig.key === key && sortConfig.direction === "asc") {
      direction = "desc";
    }
    setSortConfig({ key, direction });
  };

  const filteredData = useMemo(() => {
    let filtered = [...data];

    if (searchText) {
      filtered = filtered.filter((item) =>
        columns.some((col) =>
          item[col.key].toString().toLowerCase().includes(searchText.toLowerCase())
        )
      );
    }

    if (sortConfig.key) {
      filtered.sort((a, b) => {
        const aVal = a[sortConfig.key];
        const bVal = b[sortConfig.key];
        if (aVal < bVal) return sortConfig.direction === "asc" ? -1 : 1;
        if (aVal > bVal) return sortConfig.direction === "asc" ? 1 : -1;
        return 0;
      });
    }

    return filtered;
  }, [data, searchText, sortConfig, columns]);

  return (
    <div>
      <input
        type="text"
        placeholder="Search..."
        className="p-2 border mb-4"
        value={searchText}
        onChange={(e) => setSearchText(e.target.value)}
      />

      <table className="w-full border-collapse border">
        <thead>
          <tr>
            {columns.map((col) => (
              <th
                key={col.key}
                className="border p-2 cursor-pointer bg-gray-100"
                onClick={() => handleSort(col.key)}
              >
                {col.label}
                {sortConfig.key === col.key && (sortConfig.direction === "asc" ? " 🔼" : " 🔽")}
              </th>
            ))}
          </tr>
        </thead>
        <tbody>
          {filteredData.length === 0 ? (
            <tr>
              <td colSpan={columns.length} className="text-center p-4">
                No data found
              </td>
            </tr>
          ) : (
            filteredData.map((item, i) => (
              <tr key={i}>
                {columns.map((col) => (
                  <td key={col.key} className="border p-2">
                    {item[col.key]}
                  </td>
                ))}
              </tr>
            ))
          )}
        </tbody>
      </table>
    </div>
  );
};

export default DataTable;

import React from "react";
import DataTable from "./components/DataTable";

const App = () => {
  const sampleData = [
    { id: 1, name: "Alice", age: 24, role: "Developer" },
    { id: 2, name: "Bob", age: 30, role: "Designer" },
    { id: 3, name: "Charlie", age: 28, role: "Manager" },
    { id: 4, name: "David", age: 22, role: "Intern" },
  ];

  const columns = [
    { key: "id", label: "ID" },
    { key: "name", label: "Name" },
    { key: "age", label: "Age" },
    { key: "role", label: "Role" },
  ];

  return (
    <div className="p-4">
      <h1 className="text-xl font-bold mb-4">Employee Data Table</h1>
      <DataTable data={sampleData} columns={columns} />
    </div>
  );
};

export default App;

