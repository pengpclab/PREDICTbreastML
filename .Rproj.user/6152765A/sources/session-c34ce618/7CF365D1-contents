#' @name PREDICTbreastML
#' @title Machine-Learning Breast Cancer Survival Prediction (PREDICTbreastML)
#'
#' @description
#' `PREDICTbreastML()` generates 10-year breast cancer–specific survival
#' predictions using a machine-learning survival model trained on the largest
#' and most diverse SEER cohort. The function automatically applies
#' ancestry-specific population reweighting, constructs a survival prediction
#' task, and returns the model-estimated probability of surviving to 10 years.
#'
#' @param ext_val_data A data frame or data.table containing external validation
#'   samples. If not supplied, the built-in `toyData` dataset is used.
#' @param censor censor time
#'
#' @return A numeric vector of predicted 10-year breast cancer–specific
#'   survival probabilities.
#'
#' @import data.table
#' @import mlr3
#' @import mlr3extralearners
#' @importFrom mlr3proba TaskSurv
#' @import xgboost
#'
#' @examples
#' data(toyData)
#' PREDICTbreastML(ext_val_data = toyData,censor = 10)
#'
#' @export

PREDICTbreastML <- function(ext_val_data = toyData, censor = 10) {

  mapping_tab <- get0("mapping_tab", envir = asNamespace("PREDICTbreastML"))
  # ------------------------------------------------------------
  # add pop_weight based on ancestry
  # ------------------------------------------------------------
  ext_val_data_final <- merge(ext_val_data, mapping_tab, by = "ancestry", all.x = TRUE)

  # ------------------------------------------------------------
  # Step 3 — reorder & remove ancestry
  # ------------------------------------------------------------
  ext_val_data_final <- ext_val_data_final[, .(
    fu,
    death_br_15,
    age_diag,
    er,
    grade,
    nodes,
    pop_weight,
    pr,
    screen,
    size
  )]

  # ============================================================
  # Step 4 — build TaskSurv, load model, predict
  # ============================================================
  task_test <- TaskSurv$new(
    id = "external_val",
    backend = ext_val_data_final,
    time = "fu",
    event = "death_br_15"
  )

  load_best_model <- function() {
    readRDS(system.file("extdata", "best_model.rds", package = "PREDICTbreastML"))
  }

  best_model <- load_best_model()
  pred <- best_model$predict(task_test)

  # return survival curve index (censor year)
  survival_rate <- pred$data$distr[, censor]
  return(survival_rate)
}
